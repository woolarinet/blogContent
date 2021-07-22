---
title: '[코어 자바스크립트] 07.클래스'
category: 'javascript'
desc: '[코어 자바스크립트] 자바스크립트에서 클래스란?'
date: '2021-07-22'
thumb: 'js'
---

# 자바스크립트의 클래스
- 자바스크립트는 프로토타이 기반 언어 (*클래스의 개념이 존재하지 않는다.* )
- 따라서 클래스 관점에서 바라본 프로토타입 시스템은 예제와 같이 이해할 수 있다.
  ``` javascript
  var Rectangle = function (width, height) {  // 생성자
    this.width = width;
    this.height = height;
  };
  Rectangle.prototype.getArea = function () {  // 프로토타입 메서드
    return this.width * this.height;
  };
  Rectangle.isRectangle = function (instance) {  // 스태틱 메서드
    return instance instanceof Rectangle &&
      instance.width > 0 && instance.height > 0;
  };

  var rect1 = new Rectangle(3, 4);
  console.log(rect1.getArea());  // 12 (0)
  console.log(rect1.isRectangle(rect1));  // Error
  console.log(Rectangle.isRectangle(rect1));  // true
  ```
    - 전형적인 생성자 함수와 인스턴스이다.
    - rect1.isRectangle 함수가 에러가 나는 이유는 rect1, rect1.__proto__와  rect1.__ proto__.__ proto__ 모두에 해당 메서드가 없기 때문이다.
    - 이렇게 인스턴스에서 직접 접근할 수 없는 메서드를 **스태틱 메서드**라고 한다. (*스태틱 메서드는 생성자 함수를 this로 해야만 호출할 수 있다.* )
  위 예제에서 length 프로퍼티를 삭제하면..
  ``` javascript
  // ...
  g.push(90);
  console.log(g);  // Grade { 0: 100, 1: 80;, 2: 90, length: 3 }
  delete g.length;
  g.push(70);
  console.log(g);  // Grade { 0: 70, 1: 80, 2: 90, length: 1}
  ```
    - Grade 클래스의 인스턴스는 배열 메서드를 상속하지만 기본적으로는 일반 객체의 성질을 지니므로 삭제가 가능한 문제점이 생긴다.
    - push를 했을 때 0번째 인덱스에 70이 들어가고 length가 다시 1이 될 수 있었던 이유는 g.__proto__가 빈 배열 ([])을 가리키고 있기 때문.
    - ***위와 같이 클래스에 있는 값이 인스턴스의 동작에 영향을 미칠 경우, 클래스의 추상성을 해치게 된다.***
## 클래스가 구체적인 데이터를 지니지 않게 하는 방법
1. 프로퍼티들을 다 지우고 새로운 프로퍼티 추가할 수 없게 하기
   ``` javascript
   delete Square.prototype.width;
   delete Square.prototype.height;
   Object.freeze(Square.prototype);
   ```
2. 더글라스 크락포드 제시 방법
   ``` javascript
   var Rectangle = function (width, height) {
     this.width = width;
     this.height = height;
   };
   Rectangle.prototype.getArea = function () {
     return this.width * this.height;
   };
   var Square = function (width) {
     Rectangle.call(this, width, width);
   };
   var Bridge = function () {};
   Bridge.prototpe = Rectangle.prototype;
   Square.prototype  new Bridge();
   Object.freeze(Square.prototype);
   ```
     - Bridge라는 빈 함수를 만들고, Bridge.prototype이 Rectangle.prototype을 참조하게 한 다음, Square.prototype에 new Bridge()로 할당하면, Rectangle 자리에 Bridge가 대체하게 된다.
     - 이로써 인스턴스를 제외한 프로토타입 체인 경로상에는 더는 구체적인 데이터가 남아있지 않게 된다.
3. Object.create
   ``` javascript
   // ...
   Square.prototype = Object.create(Rectangle.prototype);
   Object.freeze(Square.prototype);
   // ...
   ```
    - subClass의 prototypedml __proto__가 SuperClass의 prototype을 바라보되, SuperClass의 인스턴스가 되지는 않으므로 앞서 소개한 두 방법보다 간단하면서 안전하다.