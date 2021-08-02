---
title: '[자료구조] 그래프'
category: 'Algorithm'
desc: '그래프 기본 개념과 다익스트라 알고리즘'
date: '2021-07-31'
thumb: 'algorithm'
---

# 그래프란? 
- 데이터 간의 연결관계를 나타내는데에 특화된 자료구조.
- 각 노드를 **정점**, 각 선을 **간선**이라 부른다.
  - 간선으로 연결된 정점들을 서로 **인접**한다고 말한다.
- 관계에 방향성이 있는 그래프를 **방향 그래프**, 관계가 상호적인 (방향성이 없는) 그래프를 **무방향 그래프**라 부른다.
  - *ex) facebook, twitter*

  ![무방향.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/algorithm/graph/2.png)![방향.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/algorithm/graph/3.png)
- 그래프를 간단히 해시테이블로 나타내보자.
  ``` javascript
  // 무방향 그래프
  const friends = {
    "Alice": [ "sunho", "Jungwoo", "Bob" ],
    "Bob": [ "Alice", "Diana", "sunho" ],
    "Diana": [ "Bob" ],
    "Jungwoo": [ "Alice" ],
    "sunho": [ "Alice", "Bob" ]
  }
  // 방향 그래프
  const followers = {
    "Alice": [ "Bob", "sunho" ],
    "Bob": [ "sunho" ],
    "sunho": [ "Bob" ],
  }
  ```
- 그래프를 순회하는 전형적인 방법은 두 가지다.
  - **너비 우선 탐색(BFS)** 과 **깊이 우선 탐색(DFS)** 이다.
    - *위 두 방법에 대해서는 따로 자세히 정리를 해볼 예정이다 :)*

## 가중 그래프
- 일반적인 그래프와 비슷하지만 그래프의 간선에 추가적인 정보(가중치)를 포함한다.
### 데이크스트라의 알고리즘 (dijkstra algorithm)
- 최단 경로 문제를 푸는 알고리즘 중 하나로 음수 가중치가 없는 그래프에서 유용한 알고리즘이다.
  1. 시작 정점을 현재 정점으로 한다.
  2. 현재 정점에 인접한 모든 정점을 확인하여 시작 정점으로부터 알려진 모든 위치까지의 가중치를 계산하고 기록한다.
  3. 시작 정점으로부터 도달할 수 있는 방문하지 않은 가장 저렴한 알려진 정점을 다음 현재 정점으로 결정한다.
  4. 그래프 내 모든 정점을 방문할 때까지 1-3 단계를 반복한다.
- 애틀랜타부터 각 도시 (보스턴, 시카고, 덴버, 엘파소)까지의 가장 저렴한 경로를 기록하기 위한 코드를 짜보자.
  ![dijstra.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/algorithm/graph/1.png)
  ``` javascript
  class City {
    constructor (name) {
      this.name = name
      // 해시 테이블로 인접 정점 표현
      this.routes = {}
      // 노드가 애틀랜타 라면 경로는 다음과 같다.
      // { boston: 100, denver: 160 }
    }
    addRoute(city, priceInfo) {
      this.routes[city] = priceInfo
    }
  }

  const atlanta = new City("Atlanta")
  const boston = new City("Boston")
  const chicago = new City("Chicago")
  const denver = new City("Denver")
  const elPaso = new City("ElPaso")
  atlanta.addRoute(boston.name, 100)
  atlanta.addRoute(denver.name, 160)
  boston.addRoute(chicago.name, 120)
  boston.addRoute(denver.name, 180)
  chicago.addRoute(elPaso.name, 80)
  denver.addRoute(chicago.name, 40)
  denver.addRoute(elPaso.name, 140)

  const dijkstra = function (start, other) {
    // route 해시 테이블은 주어진 도시로부터
    // 다른 모든 도시까지의 모든 priceInfo 데이터와
    // 도착지까지 가는데 거치는 도시를 포함한다.
    const routes = {}
    // 위 데이터는 다음과 같다.
    // { 도시: [ 가격, 원래 도시로부터의 경로 중 이 도시 바로 전에 들리는 도시 ]}
    // ex) { Atlanta: [0, 'Atlanta'], Boston: [ 100, 'Atlanta' ], Chicago: [ 200, 'Denver' ] }
    routes[start.name] = [0, start.name]
    // 위와 같이 시작도시에서 시작도시로 가는 비용은 0이다.

    // 시작 도시 외에 다른 도시까지 가는 비용 및 경로는 아직 알려지지 않았으므로
    // 데이터를 초기화할 때 그 밖의 도시는 모두 무한대 값을 할당한다.
    other.forEach((city) => (
      routes[city.name] = [ Infinity, '' ]
    ))
    // 방문한 도시를 기록한다.
    let visited = []
    // 시작도시를 현재 도시로 해서 방문을 시작
    let current = start
    // 각 도시를 방문하는 루프를 시작
    while (current) {
      // 현재 도시를 방문한다.
      visited.push(current.name)
      // 현재 도시로부터의 각 경로를 확인한다.
      for (const [ city, priceInfo ] of Object.entries(current.routes)) {
        // 시작 도시에서 다른 도시로의 경로가 route에 현재 기록된 값보다 저렴하면 업데이트 한다.
        if (routes[city][0] > priceInfo + routes[current.name][0]) {
          routes[city] = [ priceInfo + routes[current.name][0], current.name ]
        }
      }
      // 다음으로 방문할 도시를 정한다.
      current = false
      let cheapest = Infinity
      // 가능한 모든 경로를 확인한다.
      for (const [ city, priceInfo ] of Object.entries(routes)) {
        // 이 경로가 현재 도시로부터의 가장 저렴한 경로이고, 아직 방문하지 않았다면 다음으로 방문할 도시가 된다.
        if (priceInfo[0] < cheapest && visited.indexOf(city) < 0) {
          cheapest = priceInfo[0]
          other.forEach((eachCity) => {
            if (eachCity.name === city) {
              current = eachCity
            }
          })
        }
      }
    }
    return routes
  }

  const routes = dijkstra(atlanta, [ boston, chicago, denver, elPaso ])
  for (const [ city, priceInfo ] of Object.entries(routes)) {
    console.log(`# ${city}: ${priceInfo[0]}`)
    // # Atlanta: 0
    // # Boston: 100
    // # Chicago: 200
    // # Denver: 160
    // # ElPaso: 280
    // 위와같이 애틀랜타로부터 다른 모든 도시로 가는 가장 저렴한 가격을 보여준다.
  }
  ```
#### 그래프는 관계를 포함하는 데이터를 처리할 때 매우 유용한 도구이며 코드 속도를 높임과 동시에 까다로운 문제를 푸는 데 도움이 될 수 있음을 알 수 있다.

## Reference
- 누구나 자료구조와 알고리즘
- 