---
id: 2
img: javascript/js-thumb.png
title: [코어 자바스크립트] 02.실행 컨텍스트
commentor: sunho
date: 14 July 2021
tag: javascript, core-javascript
description1: 실행 컨텍스트는 실행할 코드에 제공할 환경 정보들을 모아놓은 객체로, 자바스크립트의 동적 언어로서의 성격을 가장 잘 파악할 수 있는 개념이다.
description2: 동일 환경의 코드들을 실행 시, 필요한 환경 정보들을 모아 컨텍스트를 구성하고 콜 스택에 쌓아 올린 후, 가장 위의 컨텍스트 관련 코드들을 실행하며 전체 코드의 환경과 순서를 보장한다.
descriptions:
어떠한 실행 컨텍스트가 활성화 될 때, 자바스크립트 엔진은 해당 컨텍스트와 관련된 코드들을 실행하는 데 필요한 환경 정보들을 수집해서 실행 컨텍스트 객체에 저장한다. 대표적으로 VariableEnvironment, LexicalEnvironment, ThisBinding이 있다.
'VariableEnvironment'는 현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경 정보이며 변경 사항이 반영되지 않는다. VariableEnvironment에 담기는 내용은 LexicalEnvironment와 같지만 최초 실행 시의 스냅샷을 유지한다는 점에서 차이가 있다. 실행 컨텍스트 생성 시, VariableEnvironment에 정보를 먼저 담고 이를 그대로 복사하여 LexicalEnvironment를 만들고 이후에는 LexicalEnvironment를 주로 활용한다. VariableEnvironment와 LexicalEnvironment의 내부는 environmentRecord와 outerEnvironmentReference로 구성되어 있다.
'LexicalEnvironment'는 초기엔 VariableEnvironment 와 같지만 변경 사항이 실시간으로 반영된다. 'environmentRecord'에는 현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장되며,(컨텍스트 구성 함수에 지정된 매개변수 식별자, 함수 자체, var로 선언된 변수의 식별자 등이 식별자에 해당) 컨텍스트 내부 전체를 처음부터 끝까지 훑으며 순서대로 수집한다. 여기서 hoisting이란 개념이 등장한다.
'hoisting'이란 변수 정보를 수집하는 과정을 이해하기 쉬운 방법으로 대체한 가상개념이다. environmentRecord는 현재 실행될 컨텍스트의 대상 코드 내에 어떤 식별자들이 있는지에만 관심이 있기에 호이스팅 시에 변수명만 끌어올리고 할당 과정은 원래자리에 남겨둔다.
'ThisBinding'은 익숙한 단어인데, this 식별자가 바라봐야 할 대상 객체를 뜻한다.

<스코프, 스코프 체인, outerEnvironmentReference>
'스코프'란, 식별자의 유효범위이다. 어떤 경계 A의 외부에서 선언한 변수는 A의 외부와 내부 모두 접근이 가능하지만, A 내부에서 선언한 변수는 A의 내부에서만 접근 가능하다. 이러한 스코프를 안에서부터 바깥으로 검색해나가는 것을 '스코프 체인'이라고 부른다. 그리고 이를 가능케 하는 것이 'outerEnvironmentReference' 이다. outerEnvironmentReference 는 현재 호출된 함수가 선언될 당시의 LexicalEnvironment를 참조한다.
---
