---
title: '[코어 자바스크립트] 02.실행 컨텍스트'
category: 'javascript'
desc: '[코어 자바스크립트] 실행 컨텍스트 요약 정리'
date: '2021-07-14'
thumb: 'js'
---

# 실행 컨텍스트란?
##### 실행 컨텍스트는 **실행할 코드에 제공할 환경 정보들을 모아놓은 객체**로, 자바스크립트의 동적 언어로서의 성격을 가장 잘 파악할 수 있는 개념 (클로저를 지원하는 대부분의 언어에서 유사한 개념 적용)

- 동일 환경의 코드들을 실행 시, 필요한 환경 정보들을 모아 컨텍스트를 구성하고 콜 스택에 쌓아 올린 후, 가장 위의 컨텍스트 관련 코드들을 실행
  - 전체 코드의 환경과 순서 보장
  - *동일 환경*을 구성할 수 있는 방법으로 전역공간, eval() 함수, 함수 등이 존재
``` javascript
// ------------------------------ (1)
var a = 1;
function outer() {
  function inner () {
    console.log(a) // undefined
    var a = 3;
  }
  inner(); // (2)
  console.log(a); // 1
}
outer(); // (3)
console.log(a); // 1
```
- 전역 컨텍스트와 관련된 코드들을 순차적으로 진행하다 (3)에서 outer 함수 호출
  - outer에 대한 환경 정보 수집해서 outer 실행 컨텍스트 생성 후 콜 스택에 담는다.
  - 콜 스택의 맨 위에 outer 실행 컨텍스트가 놓이면서 전역 컨텍스트와 관련된 코드 실행을 중단하고 outer 실행 컨텍스트와 관련된 코드들을 순차로 실행
  - 다시 (2)에서 inner 함수의 실행 컨텍스트가 콜 스택 가장 위에 담기면 outer 컨텍스트와 관련된 코드 실행을 중단하고 inner 함수 내부 코드를 순서대로 진행
  - inner 함수의 실행이 종료되면서 inner 실행 컨텍스트가 콜 스택에서 제거되고 outer 컨텍스트가 콜 스택 맨 위에 존재하게 되며 (2)의 다음 줄 부터 실행
  - outer 함수 실행이 끝나면 콜 스택에는 전역 컨텍스트만 남기 때문에 (3)다음 줄, a를 출력하고 콜 스택이 비워지게 된다.

- 이렇게 어떠한 실행 컨텍스트가 활성화 될 때, 자바스크립트 엔진은 해당 컨텍스트와 관련된 코드들을 실행하는 데 필요한 환경 정보들을 수집해서 실행 컨텍스트 객체에 저장한다. (*개발자가 코드를 통해 확인할 수는 없음* )  
담기는 정보들은 아래와 같다.
  - **VariableEnvironment**
    - 현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경 정보
    - 변경 사항이 반영되지 않음.
  - **LexicalEnvironment**
    - 초기엔 **VariableEnvironment** 와 같지만 변경 사항이 실시간으로 반영
  - **ThisBinding**
    - this 식별자가 바라봐야 할 대상 객체
  
  &nbsp;
## VariableEnvironment
##### **VariableEnvironment**에 담기는 내용은 **LexicalEnvironment**와 같지만 최초 실행 시의 스냅샷을 유지한다는 점에서 차이가 있다.
- 실행 컨텍스트 생성 시, **VariableEnvironment**에 정보를 먼저 담고 이를 그대로 복사하여 **LexicalEnvironment**를 만들고 이후에는 **LexicalEnvironment**를 주로 활용
- ***VariableEnvironment**와 **LexicalEnvironment**의 내부는 environmentRecord와 outerEnvironmentReference로 구성*

  &nbsp;
## LexicalEnvironment
### environmentRecord
- 현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장된다.  
(*컨텍스트 구성 함수에 지정된 매개변수 식별자, 함수 자체, var로 선언된 변수의 식별자 등이 식별자에 해당* )
  - 컨텍스트 내부 전체를 처음부터 끝까지 훑으며 순서대로 수집한다.
- **hoisting** 
  - *변수 정보를 수집하는 과정을 이해하기 쉬운 방법으로 대체한 가상개념*
  - environmentRecord는 현재 실행될 컨텍스트의 대상 코드 내에 어떤 식별자들이 있는지에만 관심 -> *따라서 호이스팅 시에 변수명만 끌어올리고 할당 과정은 원래자리에 남겨둠*
  - ``` javascript
    console.log(sum(1, 2));
    console.log(multiply(3, 4));
    function sum(a, b) {    // 함수 선언문
      return a + b;
    }
    var mul = function(a, b) {  // 함수 표현식
      return a * b;
    }
    ```
    - 이 컨텍스트를 호이스팅을 하게 되면 결과는 다음과 같다.
      ``` javascript
      var sum = function sum (a, b) {  // 함수 선언문은 전체를 호이스팅
        return a + b;
      };
      var mul;  // 변수는 선언부만 끌어올린다.
      
      console.log(sum(1, 2));
      console.log(mul(3, 4));

      mul = function (a, b) {  // 변수의 할당 부는 원래 자리에 남겨둠
        return a * b;
      }
      ```
      - 이 부분에서 함수 선언문과 함수 표현식의 극적인 차이가 발생한다.
      - sum 함수는 선언 전에 호출해도 문제 없이 실행되지만 mul 함수의 경우는 선언 후에야 호출이 가능하다.
      - 나는 개인적으로 함수 표현식을 더 선호하는 편인다. 함수 선언식은 위와 같이 선언 전에 호출이 가능하기 때문에 내 수준에서는 로직을 짤 때 너무 혼란스럽기 때문이다..ㅎ
  
  &nbsp;
### 스코프, 스코프 체인, **outerEnvironmentReference**
- 스코프란, 식별자의 유효범위이다. 어떤 경계 A의 외부에서 선언한 변수는 A의 외부와 내부 모두 접근이 가능하지만, A 내부에서 선언한 변수는 A의 내부에서만 접근 가능하다. 이러한 스코프를 안에서부터 바깥으로 검색해나가는 것을 스코프 체인이라고 부른다. 그리고 이를 가능케 하는 것이 **outerEnvironmentReference** 이다.

    
- **outerEnvironmentReference** 는 현재 호출된 함수가 선언될 당시의 LexicalEnvironment를 참조한다.
  - 예를 들어, A 함수 내부에 B 함수를 선언, 다시 B 함수 내부에 C 함수를 선언한 경우, 함수 C의 **outerEnvironmentReference** 는 함수 B의 LexicalEnvironment 를 참조한다. 함수 B의 LexicalEnvironment에 있는 **outerEnvironmentReference** 는 다시 함수 B가 선언될 당시(A)의 LexicalEnvironment를 참조한다.
  - 이처럼 연결리스트의 형태를 띠는데, 선언 시점의 LexicalEnvironment를 계속 찾아 올라가 전역 컨텍스트의 LexicalEnvironment까지 참조하게 된다.
  - *각  **outerEnvironmentReference**  는 오직 자신이 선언된 시점의 LexicalEnvironment만 참조하고 있다.*

  - ``` javascript
    // ------------------------------ (1)
    var a = 1;
    function outer() {
      function inner () {
        console.log(a) // undefined
        var a = 3;
      }
      inner(); // (2)
      console.log(a); // 1
    }
    outer(); // (3)
    console.log(a); // 1
    ```
    - 위의 이 코드를 다시 보자. inner 스코프 내부에서는 inner, outer, 전역스코프 모두 접근할 수 있지만, 식별자 a는 전역, inner함수 내부 모두 선언했기 때문에 inner 함수 내부에서 a에 접근하려 하면 무조건 inner 스코프의 LexicalEnvironment부터 검색할 수 밖에 없기 때문에 inner 스코프의 LexicalEnvifronment에 식별자 a가 존재하므로 스코프 체인 검색을 더 진행하지 않고 즉시 inner LexicalEnvironment 상의 a를 반환하게 된다. 즉, 전역 공간에서 선언한 동일한 이름의 a 변수에는 접근할 수 없는데 이를 ***변수 은닉화*** 라고 한다.

  &nbsp;
# 정리
- 실행 컨텍스트는 실행할 코드에 제공할 환경 정보들을 모아놓은 객체 (*전역 컨텍스트, eval, 함수 실행에 의한 컨텍스트 등이 존재* )
- 활성화 시점에 **VariableEnvironment, LexicalEnvironment, ThisBinding** 세가지 정보 수집
- 생성 시 **VariableEnvironment** 와 **LexicalEnvironment** 가 동일한 내용으로 구성되지만 **LexicalEnvironment** 는 함수 도중에 변경되는 사항이 즉시 반영된다. (***VariableEnvironment** 는 초기 상태 유지* )
- **VariableEnvironment** 와 **LexicalEnvironment** 는 매개변수명, 변수식별자, 선언한 함수명 등을 수집하는 environmentRecord와 직전 컨텍스트의 **LexicalEnvironment** 정보를 참조하는 outerEnvironmentReference로 구성
- 호이스팅은 environmentRecord의 수집과정을 추상화한 개념 (*실행 컨텍스트가 관여하는 코드 집단의 최상단으로 이들을 끌어올린다* )
- 스코프는 변수의 유효범위. outerEnvironmentReference는 해당 함수가 선언된 위치의 **LexicalEnvironment** 를 참조
- 전역 컨텍스트의 **LexicalEnvironment** 에 담긴 변수는 전역변수.
- 함수에 의해 생성된 실행 컨텍스트의 변수들은 모두 지역변수
- ***안전한 코드 구성을 위해 가급적 전역변수의 사용은 최소화하는 것이 좋음***
