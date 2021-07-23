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
## 클래스가 구체적인 데이터를 지니지 않게 하는 방법 (*가볍게 읽고 이해하기* )
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
4. 상위 클래스에서의 접근 수단 제공
   ``` javascript
   // ...
   SubClass.prototype.super = function (propName) {
     var self = this;
     if (!propName) return function () {
       SuperClass.apply(self, arguments);
     }
     var prop = SuperClass.prototype[propName];
     if (typeof prop !== 'function') return prop;
     return function () {
       return prop.apply(self, arguments);
     };
     // ...
   };
   ```
    - SuperClass 생성자 함수에 this.super()를 이용.
## ES6의 클래스 및 클래스 상속
   ``` javascript
   var Rectangle = class {
     constructor (width, height) {
       this.width = width;
       this.height = height;
     }
     getArea() {
       return this.width * this.height;
     }
   };
   var Square = class extends Rectangle {
     constructor (width) {
       super(width, width);
     }
     getArea () {
       console.log('size is : ', super.getArea());
     }
   };
   ```
   - (10번째 줄): class 명령어 뒤에 'extends Rectangle' 라는 내용을 추가함으로써 상속관계설정 끝
   - (12번째 줄): constructor 내부에서는 super라는 키워드를 함수처럼 사용할 수 있는데, 이 함수는 SuperClass의 constructor를 실행
   - (15번째 줄): constructor 메서드를 제외한 다른 메서드에서는 super 키워드를 마치 객체처럼 사용가능 (*이때 객체는 SuperClass.prototype을 바라보는데, 호출한 메서드의 this는 'super'가 아닌 원래의 this를 그대로 따름* )

  &nbsp;
# 마치며
- 코어자바스크립트라는 책을 추천받아 읽기 시작했는데 1회독을 한것 만으로도 상당히 깨달음을 많이 얻었다. 좀 더 근본적인 부분을 공부할 수 있어서 너무 좋았다. 현재 2회독하는 중인데 3회독한 후 다음 책으로 넘어가볼 생각이다.