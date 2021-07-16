---
title: '[코어 자바스크립트] 04.콜백 함수'
category: 'javascript'
desc: '[코어 자바스크립트] 콜백 함수, 콜백지옥, 비동기 제어'
date: '2021-07-16'
thumb: 'js'
---

# 콜백 함수
- **콜백 함수(callback function)**는 다른 코드의 인자로 넘겨주는 함수이다.
- 콜백함수를 넘겨받은 코드는 이 콜백 함수를 필요에 따라 적절한 시점에 실행  
  = *제어권과 관련이 깊다.*

## 제어권
``` javascript
var count = 0;
var cbFunc = function() {
  console.log(count);
  if (++count > 4) clearInterval(timer);
};
var timer = setInterval(cbFunc, 300);
```
- 여기서 cbFunc 함수가 콜백 함수가 된다.
cbFunc 함수는 호출주체가 사용자며 제어권 또한 사용자에게 있다.  
반면, setInterval은 호출주체와 제어권 모두 자신(setInterval)에 있다.
- ***즉, 콜백 함수의 제어권을 넘겨받은 코드는 콜백 함수 호출 시점에 대한 제어권을 가진다.***

  &nbsp;
- 또 다른 예시를 보자.
  ``` javascript
  var input = [10, 20].map(function(currentValue, index) {
    console.log(currentValue, index);
    return currentValue + 5;
  });
  console.log(input);

  // -- 실행결과 --
  // 10 0
  // 20 1
  // [15, 25]
  ```
  Array.prototype에 담긴 map 메서드는 다음과 같은 구조로 이루어져 있다.
  ``` javascript
  Array.prototype.map(callback[, thisArg])
  callback: function(currentValue, index, array)
  ```
  - map 메서드는 첫 번째 인자로 콜백 함수를 받고, 생략 가능한 두번째 인자로 콜백함수 내부에서 this로 인식할 대상을 특정할 수 있다. (*thisArg를 생략할 경우엔 전역객체가 바인딩* )
  - 배열의 각 요소를 하나씩 꺼내어 콜백 함수를 실행한다. 그 후 새로운 배열이 만들어져서 input에 담기게 된다.

# 콜백 지옥과 비동기 제어
- **콜백 지옥**은 콜백 함수를 익명 함수로 전달하는 과정이 반복되어 코드의 들여쓰기 수준이 감당하기 힘들 정도로 깊어지는 현상이다.
- **비동기 코드**는 별도의 대상에 무언가를 요청하고 그에 대한 응답이 왔을 때 비로소 어떠한 동작을 실행하도록 대기하는 행위와 관련된 코드이다.

- 현대의 자바스크립트는 웹의 복잡도가 높아진 만큼 비동기적인 코드의 비중이 예전보다 훨씬 높아짐에 따라 콜백 지옥에 빠지기도 훨씬 쉬워졌다.
  - 비동기적인 일련의 작업을 동기적으로 보이게끔 처리해주는 장치를 마련하고자 ES6에서는 Promise, Generator 등이 도입됐고, ES2017에서는 async/await가 도입됐다.
  ``` javascript
  // Promise + async/awiat
  var addPost = function (name) {
    return new Promise(function (resolve) {
      setTimeout(function () {
        resolve(name);
      }, 500)
    });
  };
  var getPost = async function () {
    var postList = '';
    var _addPost = async function (name) {
      postList += (postList ? ',' : '') + await addPost(name);
    };
    await _addPost('콜백 지옥');
    console.log(postList);
    await _addPost('비동기 제어');
  };
  getPost();
  ```
  - async/await는 가독성이 뛰어나고 사용하기 다소 쉽다.
    - 비동기 작업을 수행하고자 하는 함수 앞에 async를 표기하고, 함수 내부에서 실직적인 비동기 작업이 필요한 위치마다 await를 표기하는 것만으로 뒤의 내용을 Promise로 자동 전환하고 해당 내용이 resolve된 이후에야 다음으로 진행한다. (*Promise의 then과 흡사한 효과* )

  &nbsp;
# 정리
- **콜백 함수**는 다른 코드에 인자로 넘겨줌으로써 그 제어권도 함께 위임한 함수
- *제어권*을 넘겨받은 코드는 다음과 같은 *제어권*을 가진다.
  - 콜백 함수를 호출하는 시점을 스스로 판단
  - 콜백 함수를 호출할 때 인자로 넘겨줄 값들 및 그 순서가 정해져 있다. 이 순서를 따르지 않고 코드를 작성하면 엉뚱한 결과를 얻게된다.
  - 콜백 함수의 this가 무엇을 바라보도록 할지 정해져 있는 경우 존재 (*정하지 않은 경우는 전역객체를 바라봄* )
    - 사용자 임의로 this를 바꾸고 싶을 때는 bind 메서드를 활용하면 된다.
- 어떤 함수에 인자로 메서드를 전달해도 함수로서 실행
- 비동기 제어를 위해 콜백 함수를 사용하다 보면 콜백 지옥에 빠지기 쉽다. 이를 해결하기 위해 Promise, Generator, async/await 등의 방법들이 속속 등장 중