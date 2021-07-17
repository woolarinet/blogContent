---
title: '[코어 자바스크립트] 06.프로토타입'
category: 'javascript'
desc: '[코어 자바스크립트] 프로토타입에 대한 이해'
date: '2021-07-16'
thumb: 'js'
--- 

# 프로토타입의 개념 이해
- 자바스크립트는 prototype 기반 언어이다.
- 클래스 기반 언어의 '상속'과는 달리 프로토타입 기반 언어에서는 어떤 객체를 원형(prototype)으로 삼고 이를 복제(참조)하여 상속과 비슷한 효과를 얻는다.

 &nbsp;
  ``` javascript
  var instance = new Constructor();
  ```
- 생성자 함수(Constructor)를 new 연산자와 함께 호출 하면 Constructor에서 정의된 내용을 바탕으로 새로운 인스턴스(instance)가 생성된다.

- 이 때 instance에는 __proto__라는 프로퍼티가 자동으로 부여되는데 이 프로퍼티는 Constructor의 prototype이라는 프로퍼티를 참조한다.

- prototype 객체 내부에는 인스턴스가 사용할 메서드가 저장되는데 그렇게 되면 인스턴스에서도 숨겨진 __proto__를 통해 메서드들에 접근할 수 있게 된다.

``` javascript
var Person = function (name) {
  this._name = name;
}
Person.prototype.getName = function() {
  return this._name
};
var suzi = new Person('Suzi');
suzi.getName();  // Suzi   
                // (__proto__는 생략 가능한 프로퍼티 -> 생략하지 않는다면 undefined 출력
                //  이유는 03.this 단원에서 이미 배웠다! 계속 복습하며 공부하쟝)
var me = new Person('sunho');
me.getName(); // sunho
```
- 대표적인 내장 생성자 함수 Array를 바탕으로 다시 보자.
``` javascript
var arr = [1, 2];
console.dir(arr);
console.dir(Array);

Array.isArray(arr);  // true
arr.isArray();  // TypeError: arr.isArray is not a function
```
- 먼저 위의 출력 결과는 다음과 같다. (크롬 개발자도구)  
  ![arr.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/javascript/2.png)
  - 왼쪽은 arr 변수를, 오른쪽은 Array를 출력한 결과이다.
  - instance인 [1, 2]의 __proto__는 Array.prototype을 참조하는데, 생략이 가능하기 때문에 인스턴스가 push, pop등의 메서드를 자신의 것처럼 호출할 수 있다. 하지만 밑의 isArray나 from 등의 정적 메서드들은 프로퍼티 내부에 있지 않기때문에 호출할 수 없다. 따라서 에러가 나기에 이들은 Array 생성자 함수에서 직접 접근해야 실행이 가능하다.

## constructor 프로퍼티
``` javascript
var arr = [1, 2];
Array.prototype.constructor === Array  // true
arr.__proto__constructor === Array  // true
arr.constructor === Array  // true

var arr2 = new arr.constructor(3, 4);
console.log(arr2)  // [3, 4]
```
- 생성자 함수의 프로퍼티인 prototype 객체 내부에는 constructor라는 프로퍼티가 있다. 인스턴스의 __proto__객체 내부에도 마찬가지다. 이 프로퍼티는 단어 그대로 자기자신을 참조한다.
- 하지만 예외적인 경우를 제외하고 constructor 값을 바꿀 수 있다.
  ``` javascript
  var newConstructor = function () {
    console.log('this is new constructor!');
  };
  var dataTypes = [
    1,  // Number & false
    'test',  // String & false
    true,  // Boolean & false
    {},  // NewConstructor & false
    new Number();  // NewConstructor & false
  ];
  dataTypes.forEach(function (d) {
    d.constructor = NewConstructor;
    console.log(d.constructor.name, '&', d instanceof newConstructor)
  });
  ```
    - 이처럼 인스턴스의 생성자 정보를 알아내기 위해 constructor 프로퍼티에 의존하는게 항상 안전하진 않다. 그래도 참조하는 대상이 변경될 뿐 이미 만들어진 인스턴스의 원형이나 데이터타입이 변하진 않는다.
  
  &nbsp;
# 프로토타입 체인
- 메서드 오버라이드
  ``` javascript
  var Person = function (name) {
  this._name = name;
  }
  Person.prototype.getName = function() {
    return this._name
  };
  var suzi = new Person('Suzi');
  suzi.getName = function () {
    return '나는' + this.name;
  };
  console.log(suzi.getName());  // 나는 수지
  ```
    - 이 경우 메서드 위에 메서드를 덮어씌우게 된다. 원본을 교체하는 것이 아닌, 원본 위에 또 다른 대상을 얹는다고 생각하면 된다.
    ``` javascript
    console.log(suzi.__proto__getName());  // undefined
    ```
    - 여전히 prototype에 있는 메서드에는 접근 가능하다. 이 메서드에 접근하기 위해 this를 인스턴스를 바라보게 해주면 된다. (*call 혹은 apply를 이용해서* )
- prototype은 '객체'이다. 따라서 모든 객체의 __proto__에는 Object.prototype이 연결된다.
- 이는 즉 __proto__를 한번 더 따라가면 Object.prototype을 참조할 수 있다.
  ``` javascript
  var arr = [1, 2];
  arr(.__proto__).push(3);
  arr(.proto__)(.__proto__).hasOwnProperty(2);  // true
  ```
    - 이처럼 어떤 __proto__프로퍼티 내부에 다시 __proto__프로퍼티가 연쇄적으로 이어진 것을 **프로토타입 체인**이라하고 이 체인을 따라 검색하는 것을 **프로토타입 체이닝**이라 한다.
    
    &nbsp;
## 메서드 오버라이드와 프로토타입 체이닝
``` javascript
var arr = [1, 2];
Array.prototype.toString.call(arr);  // 1,2
Object.prototype.toString.call(arr);  // [object Array]
arr.toString();  // 1,2

arr.toString = function() {
  return this.join('_');
};
arr.toString();  // 1_2
```
  - arr 변수는 배열이므로 arr.__proto__는 Array.prototype을 참조하고 Array.prototype.__proto__는 Object.prototype을 참조한다.
  - toString 메서드는 Object.prototype에도 있다.
  - arr에 직접 toString 메서드를 부여하면 위의 arr.toString()은 Array.prototype.toString이 아닌 arr.toString이 바로 실행된다.
  
  &nbsp;
## 객체 전용 메서드의 예외상황
- 어떤 생성자 함수든 prototype은 반드시 객체이기 때문에 Object.prototype이 언제나 프로토타입 최상단에 위치한다.
- 따라서 객체에서만 사용할 메서드는 다른 데이터 타입처럼 프로토타입 객체 안에 정의할 수가 없다.
- 객체만을 대상으로 동작하는 객체 전용 메서드들은 Object.prototype이 아닌 정적 메서드로 부여한다. 다른 데이터 타입도 해당 메서드를 사용하는 것을 방지하기 위해서이다.
- 반대로 같은 이유에서 Object.prototype에는 어떤 데이터에서도 활용할 수 있는 범용적인 메서드들만 존재한다.

  &nbsp;
## 다중 프로토타입 체인
- __proto__가 가리키는 대상, 즉 생성자 함수의 prototype이 연결하고자 하는 상위 생성자 함수의 인스턴스를 바라보게 해주면 몇단계의 프로토타입 체인을 만드는 것이 가능하다.
``` javascript
var Grade = function () {
  var args = Array.prototype.slice.call(arguments);
  for (var i = 0; i < args.length; i++) {
    this[i] = args[i]
  }
  this.length = args.length;
};
var g = new Grade(100, 80)
```
  - 변수 g는 Grade의 인스턴스를 바라본다. Grade의 인스턴스는 배열의 메서드를 사용할 수 없는 유사배열객체이다. g.__proto__, 즉 Grade.prototype이 배열의 인스턴스를 바라보게 할 수 있다.
    ``` javascript
    // ...
    Grade.prototype = [];

    // 이제 g에서 직접 배열의 메서드를 사용할 수 있다.
    console.log(g);  // Grade(2) [100, 80]
    g.pop();
    console.log(g);  // Grade(1) [100]
    ```

  &nbsp;
# 정리
- 어떤 생성자 함수를 new 연산자와 함께 호출하면 Constructor에서 정의된 내용을 바탕으로 새로운 인스턴스가 생성되는데, 이 인스턴스에는 __proto__라는, Constructor의 prototype 프로퍼티를 참조하는 프로퍼티가 자동으로 부여된다.  

- __proto__는 생략 가능 속성이라 인스턴스는 Constructor의 메서드를 마치 자신의 메서드인 것처럼 호출할 수 있다.  

- Constructor.prototypedpsms constructor라는 프로퍼티가 있는데 이는 생성자 함수 자신을 가리킨다. (*이 프로퍼티는 인스턴스가 자신의 생성자 함수가 무엇인지 알고자 할 때 필요한 수단이다.* )  

- __proto__방향을 계속 찾아가면 최종적으로 Object.prototype에 당도하게 된다. 이런 식으로 __proto__안에 다시 __proto__를 찾아가는 과정을 **프로토타입 체이닝**이라 하며, 이를 통해 각 프로토타입 메서드를 자신의 것처럼 호출할 수 있다. (*접근 방식은 자신으로부터 가까운 대상부터 점차 먼 대상으로 나아간다. 원하는 값을 찾으면 검색을 중단한다.* )  

- Object.prototype에는 모든 데이터 타입에서 사용할 수 있는 범용적인 메서드만이 존재하며, 객체 전용 메서드는 여느 데이터타입과 달리 Object 생성자 함수에 스태틱하게 담겨있다.  

- **프로토타입 체인**은 반드시 1~2단계로만 이뤄지는 것이 아니라 무한대의 단계를 생성할 수 있다.