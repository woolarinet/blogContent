---
title: '[코어 자바스크립트] 05.클로저'
category: 'javascript'
desc: '[코어 자바스크립트] 클로저에 대한 이해'
date: '2021-07-16'
thumb: 'js'
keyword: 'Javascript', 'Core Javascript', 'closure'
---

# 클로저
- 어떤 함수 A에서 선언한 변수 a를 참조하는 내부함수 B를 외부로 전달할 경우 A의 실행 컨텍스트가 종료된 이후에도 변수 a가 사라지지 않는 현상
  ``` javascript
  var outer = function() {
    var a = 1;
    var inner = function () {
      return ++a;
    };
    return inner();
  }
  var outer2 = outer();
  console.log(outer2);  // 2
  ```
  - inner 함수 내부에서 외부변수 a를 사용했다.
  - inner 함수 자체를 반환하면서 outer 함수의 실행 컨텍스트가 종료될 때 outer2 변수는 outer의 리턴 값 inner함수를 참조하게 된다.
  - inner 함수에는 outer 함수의 LexicalEnvironment가 담기는데, inner 함수의 실행시점에는 outer 함수는 이미 종료된 상태인데 outer의 LexicalEnvironment에 어떻게 접근할까?
    - GC(Garbage Collector)의 동작 방식 (*어떤 값을 참조하는 변수가 하나라도 있으면 그 값은 수집 대상에 포함시키지 않는다* )으로 인해 접근이 가능해진다.
  ``` javascript
  (function () {
    var a = 0;
    var intervalId = null;
    var inner = function () {
      if (++a >= 10) {
        clearInterval(intervalId);
      }
      console.log(a);
    };
    intervalId = setInterval(inner, 1000);
  })();
  ```
    - 이 경우또한 두 지역변수를 참조하는 내부함수를 외부에 전달했기에 클로저라고 할 수 있다.
- 메모리 누수의 위험을 이유로 클로저 사용을 조심해야한다는 주장이 있는데, 메모리 소모는 클로저의 **본질적 특성**일 뿐이다.
  - 의도적으로 참조 카운트를 0이 되게 하지 않게 설계한 경우는 '누수'라고 볼 수 없다.
  - 따라서 클로저를 사용할 경우에는 필요성이 사라진 시점에 메모리를 소모하지 않게 관리해주면 된다.
    - 즉, 참조 카운트를 0으로 만들면 되는 것 (*null 혹은 undefined 할당* )
    ``` javascript
    (function () {
    var a = 0;
    var intervalId = null;
    var inner = function () {
      if (++a >= 10) {
        clearInterval(intervalId);
        inner = null;  // inner 식별자의 함수 참조를 끊음
      }
      console.log(a);
    };
    intervalId = setInterval(inner, 1000);
    })();
    ```
# 클로저 활용 사례
1. 콜백 함수 내부에서 외부 데이터를 사용할 때
   ``` javascript
   var fruits = ['apple', 'banana', 'peach'];
   var $ul = documnet.createElement('ul');

   fruits.forEach(function (fruit) {
     var $li = document.createElement('li');
     $li.innerText = fruit;
     $li.addEventListener('click', function() {
       alert('your choice is ' + fruit);
     });
     $ul.applendChild($li);
   });
   document.body.appendChild($ul);
   ```
     - addEventListener에 넘겨준 콜백함수에서 fruit이라는 외부 변수를 참조하므로 클로저가 존재한다. 클릭 이벤트에 의해 앞의 콜백함수의 L.E를 참조하게 된다.
2. 정보 은닉 (접근 권한 제어)
   ``` javascript
   // 자동차 경주 게임 코드

   var createCar = function () {
     var fuel = Math.ceil(Math.random() * 10 + 10);
     var power = Math.ceil(Math.random() * 3 + 2);
     var moved = 0;
     return {
       get moved () {
         return moved;
       },
       run: function () {
         var km = Math.ceil(Math.random() * 6);
         var wasteFuel = km / power;
         if (fuel < wasteFuel) {
           console.log('이동불가');
           return;
         }
         fuel -= wasteFuel;
         moved += km;
         console.log(km + 'km 이동 (총 ' + moved + 'km). 남은 연료: ' + fuel);
       }
     };
   };
   var car = createCar();
   ```
     - createCar 함수를 실행함으로서 객체를 생성
     - fuel, power 변수는 비공개 멤버로 지정하여 외부에서의 접근을 제한하고 moved 변수는 getter만을 부여함으로써 읽기 전용 속성을 부여했다.
     - 추가로 어뷰징까지 막기 위해서는 return 전에 Objct.freeze()를 이용할 수 있다.
3. 부분 적용 함수
- **부분 적용 함수**란 n개의 인자를 받는 함수에 미리 m개의 인자만 넘겨 기억시키고 나중에 남은 인자를 넘겨 실행 결과를 얻을 수 있게끔 하는 함수.  

   ``` javascript
   var debounce = function (eventName, func, wait) {
     var timeoutId = null;
     return function (event) {
       var self = this;
       console.log(eventName, 'event 발생');
       clearTimeout(timeoutId);
       timeoutId = setTimeout(func.bind(self, event), wait);
     };
   };

   var moveHandler = function (e) {
     console.log('move event 처리');
   };
   var wheelHandler = function (e) {
     console.log('wheel event 처리');
   };
   document.body.addEventListener('mousemove', debounce('move', movedHandler, 500));
   document.body.addEventListener('mousewheel', debounce('wheel', wheelHandler, 700));
   ```
   - 최초 event가 발생 시 timeout 대기열에 'wait시간 뒤에 func를 실행'하라는 내용이 담긴다.
   - wait시간이 경과하기 전에 event가 발생하면 앞서 저장한 대기열을 초기화하고 새로운 대기열을 등록
   - 결국 각 이벤트가 바로 이전 이벤트로부터 wait 시간 이내에 발생하는 한 마지막에 발생한 이벤트만이 초기화되지 않고 무사히 실행된다.
   - 여기서 클로저로 처리되는 변수는 eventName, func, wait, timeoutId가 있다.
4. 커링 함수
  - 여러 개의 인자를 받는 함수를 하나의 인자만 받는 함수로 나눠 순차적으로 호출될 수 있게 체인 형태로 구성한 것
    - 한번에 한 인자만 전달하는 것을 원칙으로 함.
    - 마지막 인자가 전달되기 까지 원본 함수가 실행되지 않는다.
    ``` javascript
    // 지연 실행
    var getInfo = function(baseUrl) {
      return function (path) {
        return function (id) {
          return fetch(baseUrl + path + '/' + id);
        };
      };
    };
    // ES6
    var getInfo = baseUrl => path => id => fetch(baseUrl + path + '/' + id);
    ```
    - 당장 필요한 정보만 받아서 전달하고, 또 필요한 정보가 들어오면 전달하는 식으로 결국 마지막 인자가 넘어갈 때까지 함수 실행을 미루는 데 이를 **지연실행**이라 한다.
    - 예를 들어, REST API를 이용할 경우 baseUrl은 몇개로 고정되지만 나머지 path나 id 값은 매우 많을 수 있다.
    - 이러한 상황에서 공통적인 요소(baseUrl)는 먼저 기억시켜두고 특정한 값(id)만으로 서버 요청을 수행하는 함수를 만들어두는 편이 개발 효율성이나 가독성 측면에서 더 좋다.

  &nbsp;
# 정리
- **클로저**란 어떤 함수에서 선언한 변수를 참조하는 내부함수를 외부로 전달할 경우, 함수의 실행 컨텍스트가 종료된 후에도 해당 변수가 사라지지 않는 현상
- 내부함수를 외부로 전달하는 방법에는 함수를 return하는 것과 콜백으로 전달하는 경우가 있다.
- **클로저**는 본질이 메모리를 계속 차지하는 개념이므로 더이상 사용하지 않게 된 클로저는 메모리를 차지하지 않도록 관리해줄 필요가 있다.

