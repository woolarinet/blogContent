---
title: '[코어 자바스크립트] 03.this'
category: 'javascript'
desc: '[코어 자바스크립트] this 전반적인 이해'
date: '2021-07-15'
thumb: 'js'
---

# 전역 공간에서의 this
``` javascript
// 브라우저 환경
console.log(this);  // { alert: f(), atob: f(), blur: f(), btoa: f(), ... }
console.log(window);  // { alert: f(), atob: f(), blur: f(), btoa: f(), ... }
console.log(this === window);  // true

// Node.js 환경
console.log(this);  // { process: { title: 'node', version: 'v10.13.0',... } }
console.log(global); // { process: { title: 'node', version: 'v10.13.0',... } }
console.log(this === global); // true
```
- 전역 공간에서 this는 전역 객체를 가리킨다. 전역 컨텍스트를 생성하는 주체가 전역 객체이기 때문.
- 자바스크립트의 모든 변수는 특정 객체의 프로퍼티로서 동작한다. 따라서 전역변수를 선언하는 경우는, 자바스크립트 엔진은 이를 전역 객체의 프로퍼티로 할당한다.
  ``` javascript
  var a = 1;
  console.log(a);  // 1
  console.log(window.a);  // 1
  console.log(this.a);  // 1
  ```
  - 이 경우, a를 직접 호출할 때, 스코프 체인에서 a를 검색하다가 마지막에 도달하는 전역 스코프의 LexicalEnvironment, 즉 전역객체에서 해당 프로퍼티 a를 발견해서 그 값을 반환하기 때문이다. (*단순하게 (window.)이 생략된 것이라 여겨도 무방* )
  - 따라서 ***대부분의 경우***에는 전역 공간에서 var로 변수를 선언하는 대신, window의 프로퍼티에 직접 할당하더라도 결과적으로 var로 선언한 것과 똑같이 작동한다.
  - 다른 경우가 하나 있는데, delete 명령에 대한 경우다.
      ``` javascript
      // var로 선언하는 경우

      var a = 1;
      delete window.a;  // false
      console.log(a, window.a, this.a);  // 1 1 1

      // window 프로퍼티로 할당하는 경우

      window.c = 1;
      delete window.c;  // true
      console.log(c, window.c, this.c);  // Uncaught ReferenceError: c is not defined
      ```
    - 사용자가 의도치 않게 삭제하는 것을 방지하는 차원에서 마련한 나름의 방어 전략이라 해석된다.
- **즉 전역변수를 선언하면 자바스크립트 엔진이 이를 자동으로 전역객체의 프로퍼티로 할당하면서 추가적으로 해당 프로퍼티의 configureable 속성을 false로 정의하는 것이다.**
##### 이처럼 var로 선언한 전역변수와 전역객체의 프로퍼티는 호이스팅 여부 및 configurable 여부에서 차이를 보인다.

  &nbsp;
# 메서드로서 호출할 때 내부에서의 this  
  &nbsp;
## 함수로서 호출, 메서드로서 호출
  ``` javascript
  var func = function(x) {
    console.log(this, x;)
  };
  func(1); // Window { ... } 1

  var obj = {
    method: func
  };
  obj.method(2);  // { method: f } 2
  obj['method'](2)  // { method: f } 2
  ```
  - 점 표기법 혹은 대괄호 표기법, 어떤 경우든 함수를 호출 시 앞에 객체가 명시되어 있는 경우는 메서드로 호출한 것 그렇지 않은 경우는 함수로 호출.

### 메서드 내부에서의 this
- this에는 호출한 주체의 정보가 담긴다. 함수를 메서드로서 호출한다면 호출 주체는 바로 함수명 앞의 객체가 된다. 명시된 객체가 곧 this인 것.
  
  &nbsp;
# 함수로서 호출할 때 내부에서의 this
- 함수를 함수로서 호출할 때에는 this가 지정되지 않는다.
- this에는 호출한 주체의 정보가 담겨야하는데 함수로서 호출하는 것은 호출 주체를 명시하지 않고 개발자가 직접 코드에 관여해서 실행한 것이기 때문에 *호출 주체의 정보를 알 수 없다.*
- ***하지만 이는 설계상 오류라는 지적이 있다.***

## 메서드 내부함수에서의 this

``` javascript
var obj1 = {
  outer: function () {
    console.log(this);  // (1) obj1
    var inner = function () {
      console.log(this);  // (2) Window (3) obj2
    };
    inner();
    var obj2 = {
      innerMethod: inner
    };
    obj2.innerMethod();
  }
};
obj1.outer()
```
- obj1.outer 함수를 실행하면, 처음은 호출주체인 obj1의 객체 정보를 출력한다.
- 그 다음 inner함수를 함수표현식으로 선언해주고 호출하게되면, 호출 주체가 없기에 스코프 체인상의 최상위 객체인 전역객체가 바인딩 된다.
- 그리고 obj2.innerMethod를 호출하면 메서드로서의 호출로 호출주체인 obj2 객체 정보가 출력된다.

> 메서드 내부 함수의 this를 *우회하는 방법*
 ``` javascript
 var obj = {
   outer: function () {
     console.log(this);  // (1) { outer: f }
     var inner1 = function () {
       console.log(this);  // (2) Window { ... }
     };
     inner()
     
     var self = this;
     var inner2 = function () {
       console.log(self);  // (3) { outer: f }
     };
     inner();
   }
 };
 obj.outer();
 ```
 > - 대표적인 방법으로 변수를 활용하여 우회할 수 있다.
 > - 상위 스코프의 this를 변수에 저장하여 내부함수에서 활용하기 위해 사용한다.

## this를 바인딩하지 않는 함수
- ES6에서는 함수 내부에서 this가 전역객체를 바라보는 문제를 보완하고자 **arrow function**을 도입하였다.
  - **arrow function**은 실행 컨텍스트를 생성할 때 this 바인딩 과정 자체가 빠지게 되어, 상위 스코프의 this를 그대로 활용할 수 있다.
  - 이 경우에는 '우회법'이 불필요하다 (*ES5 환경에서는 arrow function을 사용할 수 없음* )

# 생성자 함수 내부에서의 this
- 자바스크립트는 함수에 생성자(클래스)로서의 역할을 함께 부여.
  - new 명령어와 함께 함수를 호출하면 해당 함수가 생성자로서 동작한다.
  - 그리고 그 경우 내부의 this는 새로 만들 구체적인 인스턴스 자신이 된다.

# 명시적으로 this를 바인딩하는 방법  
  &nbsp;
## 1. call 메서드
  ``` javascript
  Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])

  var func = function (a, b) {
    console.log(this, a, b)
  };

  func(1, 2);  // Window { ... } 1 2
  func.call({ x: 1 }, 4, 5)  // { x: 1 } 4 5
  ```
  - call 메서드는 메서드의 호출 주체인 함수를 즉시 실행하도록 하는 명령.
    - 이때 첫번째 인자를 this로 바인딩하고, 이후의 인자들을 호출할 함수의 매개변수로 적용.
    - 함수를 그냥 실행하면 this는 전역객체를 참조하지만 call 메서드를 이용하면 임의의 객체를 this로 지정할 수 있다.
## 2. apply 메서드
  ``` javascript
  Function.prototype.apply(thisArg[, argsArray])

  var func = function (a, b) {
    console.log(this, a, b)
  };

  func.apply({x: 1}, [4, 5]);  // { x: 1 } 4 5
  ```
  - apply 메서드는 call 메서드와 기능적으로 완전 동일
  - apply 메서드는 두번째 인자를 배열로 받아 그 배열의 요소들을 호출할 함수의 매개변수로 지정한다는 점만 다르당.
## 3. bind 메서드
  ``` javascript
  Function.prototype.bind(thisArg[, arg1[, arg2[, ...]]])

  var func = function (a, b) {
    console.log(this, a, b)
  };

  func(1, 2);  // Window { ... } 1 2

  var bindFunc = func.bind({ x: 1 })
  bindFunc(3, 4);  // { x: 1 } 3 4
  var bindFunc2 = func.bind({ x: 1 }, 5);
  bindFunc2(6); // { x: 1 } 5 6
  ```
  - ES5에서 추가된 기능으로 call과 비슷하지만 즉시 호출하지는 않고 넘겨받은 this 및 인수들을 바탕으로 새로운 함수를 반환하기만 한다.
  - 다시 새로운 함수를 호출할 때 인수를 넘기면 기존 bind 메서드를 호출할 때 전달했던 인수들의 뒤에 이어서 등록된다.
  - 독특한 성질이 하나 있는데 name프로퍼티에 bound라는 접두어가 붙는다. 이를 통해 코드를 추적하기에 다소 수월해진 면이 있다.
    ``` javascript
    // ...
    console.log(bindFunc.name);  // bound func
    ```
## 4. 별도의 인자로 this를 받는 경우(콜백 함수 내에서의 this)
  ``` javascript
  var price = {
    sum: 0,
    count: 0,
    add: function () {
      var args = Array.prototype.slice.call(arguments);
      args.forEach(function (entry) {
        this.sum += entry;
        ++this.count;
      }, this);
    },
    average: function () {
      return this.sum / this.count;
    }
  };
  price.add(10, 20, 30);
  console.log(price.sum, price.count, price.average());  // 60 3 20
  ```
  - price 객체 내의 add 메서드는 arguments를 배열로 변환하여 args변수에 담는다.
  - 이 배열을 순회하며 콜백함수를 실행하는데 내부의 this는 forEach 함수의 두번째 인자로 전달해준 this가 바인딩 된다.
  - 10, 20, 30을 인자로 삼아 add 메서드를 호출하면 이 세 인자를 배열로 만들어 forEach 메서드가 실행된다.
  - 콜백함수 내부에서의 this는 add 메서드에서의 this가 전달된 상태이므로 그대로 price를 가리키고 있다.
  - 이 밖에도 thisArg를 인자로 받는 메서드들 중 몇개의 예시이다.
    ``` javascript
    Array.portotype.forEach(callback[, thisArg])
    Array.portotype.map(callback[, thisArg])
    Array.portotype.filter(callback[, thisArg])
    Array.portotype.some(callback[, thisArg])
    Array.portotype.find(callback[, thisArg])
    // ...
    ```

  &nbsp;
# 정리
### 1. 명시적 this 바인딩이 없을 때
   - 전역공간에서의 this는 전역객체(브라우저에서는 window, Node.js에서는 global)를 참조
   - 함수를 메서드로서 호출한 경우 this는 메서드 호출 주체를 참조
   - 함수를 함수로서 호출한 경우 this는 전역객체 참조 (*메서드 내부함수도 동일* )
   - 콜백 함수 내부의 this는 콜백 함수의 제어권을 넘겨받은 함수가 정의한 바에 따름 (*정의하지 않으면 전역객체 참조* )
   - 생성자 함수의 this는 생성될 인스턴스 참조

### 2. 명시적 this 바인딩이 있을 때
   - call, apply 메서드는 this를 명시적으로 지정하며 함수 또는 메서드 호출
   - bind 메서드는 this 및 함수에 넘길 인수를 일부 지정하여 새 함수 생성
   - 요소를 순회하며 콜백함수를 반복호출하는 메서드는 별도의 인자로 this를 받기도 한다~ 
