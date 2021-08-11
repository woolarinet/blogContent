---
title: 'Mocha와 Jest의 차이점'
category: 'Node.js'
desc: 'mocha vs Jest'
date: '2021-08-11'
thumb: 'node'
---

# Jest
- Jest는 자바스크립트 테스팅 프레임워크로, 페이스북에서 유지보수를 하고 있다.
- 단순함을 중점으로 디자인 되었으며, 큰 웹서비스를 지원한다.
- 주로 Babel, TypeScript, Node.js, React, Angular, Vue.js 등을 이용한 프로젝트와 함께 이용한다.
- Jest는 다른 assertion 라이브러리 없이 내장 함수만으로 테스트가 가능하다.
- 단위 테스트에 초점이 맞춰져 있기에 간결하지만 논리적 연결이나 분리 지원은 되지않는다. 따라서 외부 라이브러리를 import 하여 테스트 하는 경우에도 매 테스트마다 import를 해주어야 하기 때문에 실행속도가 비교적 느리다.


# Mocha
- Mocha는 Node.js 프로그램을 위한 테스트 프레임워크로, 비동기 테스트, 테스트 커버리지를 비교할 수 있고, 대부분의 assertion 라이브러리와 호환이 가능하다.
- 진입장벽이 다소 높은 편이지만 논리적 연결, 분리 지원이 가능하다.
- 단위 테스트 뿐만 아니라 E2E 테스트나 기능테스트 또한 가능하기에 높은 유연성을 가지고 실행속도가 빠른 편이다.
- 추가 라이브러리를 이용하여야 그 효율성을 발휘할 수 있다.

## Implementation Comparisons

### Mocha
