---
title: 'Mocha와 Jest의 차이점'
category: 'javascript'
desc: 'mocha vs Jest'
date: '2021-08-11'
thumb: 'js'
keyword: 'Javascript', 'Test Framework', 'Mocha', 'Jest', 'TDD', 'BDD'
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
- 외부 라이브러리를 추가로 이용하여야 그 효과를 발휘할 수 있다.

## Implementation Comparisons

  &nbsp;
### 시나리오1 - 단위테스트 (로그인 유저 ID 체크)

#### Mocha

``` javascript
import { expect, should } from 'chai';
import loginService from './loginService';

describe('loginService', () => {
    it('should return true if valid user id', () => {
        expect(loginService.isValidUserId('abc123')).to.be.true;
    });
};
```

#### Jest
``` javascript
import loginService from './loginService';

describe('loginService', () => {
    it('should return true if valid user id', () => {
        expect(loginService.isValidUserId('abc123')).toBeTruthy();
    });
};
```
  &nbsp;
### 시나리오2 - 기능테스트 (사용자가 정보를 얻음)

#### Mocha
``` javascript
import { expect, should } from 'chai';
import sinon from 'sinon';
import loginService from './loginService';
import userRepository from '../repositories/user';

describe('loginService', () => {
    it('should fetch a user profile given a user id', () => {
        const expectedReturnObject = {
            id: 'abc123',
            username: 'sunho'
        };
        const getStub = sinon.stub(userRepository, 'get');
        getStub.returns(expectedReturnObject);

        const response = loginService.getUserProfile(expectedReturnObject.id);
        expect(response).to.equal(expectedReturnObject);
        expect(getStub.calledOnce).to.be.true;
    });
};
```
- Mocha 에서는 sinon과 같은 라이브러리를 이용하여 가짜객체를 생성해 테스트에 사용한다.

#### Jest
```` javascript
import loginService from './loginService';
import userRepository from '../repositories/user';

const mockGet = jest.fn(() => {
    return {
            id: 'abc123',
            username: 'sunho'
        };
});

jest.mock('../repositories/user', () => {
    return {
      get: mockGet
    };
});

describe('loginService', () => {
    it('should fetch a user profile given a user id', () => {
        const expectedReturnObject = {
            id: 'abc123',
            username: 'sunho'
        };

        const response = loginService.getUserProfile(expectedReturnObject.id);
        expect(response).toEqual(expectedReturnObject);
        expect(mockGet).toHaveBeenCalledOnce();
    });
};
````
- 반면 Jest에서는 내장함수를 이용하여 가능하다.

이 부분을 비교해보았을 때에는, 3개의 라이브러리를 이용한 테스트와 하나의 라이브러리만을 이용한 테스트가 더 부담없이 느껴질 수 있다고 생각한다 :)

## Npmtrends
- npm trends에 따르면 한달 간의 두 라이브러리의 통계는 다음과 같다.
### Downloads
- Mocha: 약 30만
- Jest: 약 10만
- 누적 다운로드 수는 Jest가 압도적으로 많다.

### Issues
- Mocha: 약 280개
- Jest: 약 1500개

### Updated
- Mocha: 18일 전
- Jest: 한달 전

## 마치며
- 사실 프론트에서는 Jest, Node 서버에서는 Mocha를 이용하며 비교하면 더욱 좋겠지만, 현재 리팩토링 부터 시작해서 우선순위가 많이 앞서있는 일들이 많은 우리 팀에서는 최대한 하나로 통일하여 서로 의견을 주고받으면서 효율적인 코드를 작성하는 게 우선이다 보니 비교를 하게 되었다.
- 정리를 하면서도 솔직히 사소한 부분 외의 차이는 잘 못느끼겠지만 (역량이 부족한 까닭) **결국 둘 다 같은 목적을 공유한다고 생각한다. 접근 방법이 다를 뿐.**
- 진입 장벽이 비교적 낮고 단순하게 빠른 이용을 하기 위해선 Jest지만, 앞으로의 신규 프로젝트나 이때까지의 개발 습관을 생각했을 때는 장기적으로 Mocha가 효율적이라고 개인적으로 판단했다 :)

## References
- [dev.to/willholmes]
- [npm-trend]

[dev.to/willholmes]: https://dev.to/willholmes/why-i-think-jest-is-better-than-mocha-chai-78l

[npm-trend]: https://www.npmtrends.com/jest-vs-mocha