---
title: '실무에 도입하기 위한 mocha와 chai 라이브러리'
category: 'Node.js'
desc: 'mocha 사용법 및 주요 문법 정리'
date: '2021-08-06'
thumb: 'node'
---

# Mocha, chai
- 테스트기반 프레임워크 Mocha와 assertion 라이브러리 chai를 실무에 도입하기 위해 공식문서를 보고 사용법을 익혀보았다.

- 다음과 같은 개발 순서를 바탕으로 익혔다.
  1. 명세서 초안 작성 (코드 작성 및 기본적인 테스트)
  2. Mocha를 이용해 명세서를 실행 (테스트를 모두 통과해 에러가 더이상 출력되지 않을 때까지 코드 수정)
  3. 모든 테스트를 통과하는 코드 초안 완성
  4. 명세서에 유스케이스 추가 (실패하는 테스트 나타나기 시작)
  5. 2.으로 돌아가 테스트를 모두 통과할 때까지 코드 수정
  6. 기능이 완성될 때까지 2~5단계 반복

## Installation
``` javascript
npm i mocha --save-dev
npm i chai --save-dev
```

## Use
- 테스트 과정에서 명심해야 할건, 테스트는 명확한 입력값, 출력값과 함께 여러 블록으로 쪼개 작성하는 것이 좋다.
- 먼저 테스트할 코드들을 하나의 모듈안에 담은 코드를 작성했다.
  ``` javascript
  // /func.js
  const test_function = {};

  test_function.sum = function (a, b) {
  return a + b;
  };

  test_function.make_msg = function (msg, data, req) {
    result = {"message": msg, "data": data, "token": req.token };
    return result;
  }

  module.exports = test_function;
  ```
- 그 후 테스트 할 파일을 생성하고, 기본형 데이터부터 차례대로 테스트를 해보았다.
- 여기서, chai의 많은 프로퍼티 중 expect만을 사용하였으며, 앞으로 실무에서 실제 적용하는 과정에서 다른 프로퍼티도 사용할지, expect만으로 충분할지 판단해 볼 예정이다.
``` javascript
// /mocha.spec.js

const axios = require('axios')
const { expect } = require('chai');
const test_function = require('./func');

// 기본형 데이터와 비교: expect(return_val).to.equal(expected_val);
describe("a + b", function () {
  it("1 + 2 = 3", function () {
    expect(test_function.sum(1, 2)).to.equal(3);
  });

  // 같지 않음을 검증
  it("3 + 2 !== 5", function () {
    expect(test_function.sum(3, 2)).to.not.equal(7);
  });

  it("2 + 7 = 9", function () {
    expect(test_function.sum(2, 7)).to.equal(9);
  });

  //it.only -> 이 많은 it들 중에 얘 하나만 실행함
  it.only("only test 1 + 1 = 2", function () {
    expect(test_function.sum(1, 1)).to.equal(2);
  });

  // it 블록 자동화
  function autoTest (a, b) {
    let expected = a + b;
    it(`${a} + ${b} = ${expected}`, function () {
      expect(test_function.sum(a, b)).to.equal(expected);
    });
  };

  for (let i = 1; i <= 5; i++) {
    const a = Math.floor(Math.random() * 100);
    const b = Math.floor(Math.random() * 100);
    autoTest(a, b);
  };
});
```
  - it을 수동적으로 추가하여 이해를 시작하였고, for문을 이용하여 it을 자동으로 생성하여 랜덤 값을 넣어 테스트를 해보기도 했다. 기본형데이터에 한해서는 반복문을 통한 테스트가 굉장히 효율적일 것 같다.
``` javascript

// 객체과 비교: expect(return_val).to.deep.equal(expected_val);
describe("DACON_make_msg", function () {
  it("hello world and array, strings;", function () {
    let input = {
      msg: 'hello world',
      data: [1, 2, 3],
      token: 'dacon'
    };
    expect(
      test_function.make_msg(input.msg, input.data, { token: input.token })
    ).to.deep.equal({ "message": input.msg, "data": input.data, "token": input.token, "status": 1 });
  });

  it("dacon arr str", function () {
    let input = {msg: "dacon", data: [2, 3, 4], token: "sunho"};
    expect(
      test_function.make_msg(input.msg, input.data, { token: input.token })
    ).to.deep.equal({ "message": input.msg, "data": input.data, "token": input.token, "status": 1 });
  });
});

// 참조형 데이터
describe("property", function () {
  // object
  it("have_own_property", function () {
    expect({ jungwoo: "da" }).to.have.own.property("jungwoo");
  });
  it("have_all_keys", function () {
    expect({ jungwoo: 1, dacon: 2 }).to.have.all.keys('jungwoo', 'da');
  });
  it("have_any_keys", function () {
    expect({ jungwoo: 1, dacon: 2 }).to.have.any.keys('a', 'da');
  });

  //array
  it("include", function () {
    expect([1, 2, 3]).to.include(1);
  });
  it("deep_include", function () {
    expect([{ a: 1 }]).to.deep.include({ a: 1 });
  });

});
```

``` javascript
// 비교 연산 - Above, Least, Below, Most, Within
describe("Comparison Operation", function () {
  const comp = [1, 3, 5];
  // 초과
  it("ABOVE", () => {
    expect(Math.max(...comp)).to.above(1);
  });
  // 이상
  it("LEAST", () => {
    expect(Math.max(...comp)).to.least(5);
  });
  // 미만
  it("BELOW", () => {
    expect(Math.max(...comp)).to.below(8);
  });
  // 이하
  it("MOST", () => {
    expect(Math.max(...comp)).to.most(5);
  });
  // 범위 (양 끝값 포함)
  it("WITHIN", () => {
    expect(comp.length).to.within(1, 10);
  });
});

// Type 체크
describe("TYPE_CHECK", function () {
  it("string", function () {
    expect("hi").to.be.a("string");
  });
  it("object", function () {
    expect({ hello: "world" }).to.be.an("object");
  });
  it("null", function () {
    expect(null).to.be.a("null");
  });
  it("undefined", function () {
    expect(undefined).to.be.an("undefined");
  });
  it("error", function () {
    expect(new Error).to.be.an("error");
  });
  it("Promise", function () {
    expect(Promise.resolve()).to.be.a('promise');
  });
  it("float_array", function () {
    expect(new Float32Array).to.be.a("float32array");
  });
  it("symbol", function () {
    expect(Symbol ()).to.be.a("symbol");
  });
});

// Error 체크
describe("ERROR_CHECK", function () {
  const error = function () {
    throw new TypeError("DACON DACON");
  };
  it("ERROR", function () {
    expect(error).to.throw(TypeError, "DACON DACON");
  });
});
```

``` javascript

// 비동기 제어
describe("ASYNC TEST", function() {
  it("async", async function () {
    try {
      const response = await axios.get("https://sunhodev.com/api/blog");
      console.log(response.status);
    } catch (err) {
      console.log(err)
    }
  });
});


// life_cycle - before/after & beforeEach/afterEach
describe("LIFE_CYCLE", function () {
  before(() => console.log("=====BEFORE TEST CYCLE====="));
  after(() => console.log("=====AFTER TEST CYCLE====="));

  beforeEach(() => console.log("\nBEFORE EACH TEST\n"));
  afterEach(() => console.log("\nAFTER EACH TEST\n"));

  it("===TEST_1===", () => console.log("THIS IS TEST1"));
  it("===TEST_2===", () => console.log("THIS IS TEST2"));
});
```
  - 제일 중요한 비동기제어 코드를 테스트하는 걸 익혀봄으로써 간단한 부분까지는 적용시킬 수 있을 것 같다.
  - 아직 사용하지 않은 문법/함수 들은 추후에 필요 시마다 공식문서를 참고하며 볼 예정이다.

## Test
``` javascript
mocha -w mocha.spec.js
```
- -w 옵션을 주면 테스트 코드를 수정할 때마다 자동으로 테스트를 진행해준다.

## Reference
- <https://www.chaijs.com/>
