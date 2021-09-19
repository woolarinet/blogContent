---
title: '스케줄러 사용하기'
category: 'Node.js'
desc: '스케줄러 모듈 비교 및 내가 선택한 node-schedule 사용법'
date: '2021-08-04'
thumb: 'node'
keyword: 'Node.js', 'scheduler', 'node-schedule', 'npm'
---

# 스케줄러 사용 계기
- 이번에 스케줄러를 사용하여 특정 요일마다 특정 코드가 작동하는 기능을 구현하는 업무를 맡게 되었다.
- 우선 로컬에서 setInterval을 통해 특정 주기마다 함수가 잘 동작하는지 테스트 해보았다.
   ``` javascript
   const testFunc = require('../scheduler');
   const intervalTest = async function () {
       return await new Promise(resolve => {
           setInterval(async () => {
               console.log(testFunc());
               resolve('foo');
           }, 300000);
       })
   };
   intervalTest();
   ```
- 5분 주기로 로그와 테이블을 확인하며 정상작동함을 확인하였고, 이제 모듈을 선택해야하는 과정에 왔다.

# node-schedule vs node-cron vs agenda
- 우선 중점적으로 다음의 사항들을 비교하였다.

### 유지보수
- node-schedule: 3명
- node-cron: 2명
- agenda: 11명
### opened issues
- node-schedule: 약 112개
- node-cron: 약 70개
- agenda: 약 213개
### 주간 다운로드 수 
- node-schedule: 약 78만
- node-cron: 약 35만
- agenda: 약 4만
### 최근 업데이트
- node-schedule: 6달 전
- node-cron: 5달 전
- agenda: 2달 전

## node-schedule 선택!
- 우선 agenda는 인원이 많은 것에 비해 오픈이슈가 너무 많았다. 주간 다운로드 수도 적기 때문에 문제가 생기면 내가 대처하기 어려울거라 판단하고 제일 먼저 제외하였다.
- node-schedule과 node-cron은 큰 차이는 없었지만, node-schedule가 아무래도 사람들 간의 소통도 활발하고 특정 날짜를 지정할 수 있음에 빠르게 적용시킬 수 있을거라 판단하여 node-schedule로 선택하였다.

## Installation
``` javascript
npm i node-schedule
```

## Use
``` javascript
// test.js
const schedule = require('node-schedule');

const test = function () {
  let cnt = 1;
    // 맨 앞의 별부터 '초 분 시 일 월 요일' 순으로 설정합니다.
    // ex) '1 * * * * *' 매 분 1초마다 실행
    // ex) '*/5 * * * * *' 5초 간격마다 실행
    // ex) '1 30 4 * * 0' 매주 일요일 오전 4시 30분 1초에 실행 (0 or 7: 일요일을 뜻합니다.)
  schedule.scheduleJob('1 */5 * * * *', async () => {
    console.log('hi ~ cnt: ' + cnt);
    cnt++;
  });
};

module.exports = test;

// app.js
const test_scheduler = require('./test');

//... (생략)
server.listen(port, host, () => {
  test_scheduler();
});
// ...
```

![swagger.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Node.js/scheduler/1.png)

- 위와 같이 함수를 작성한 후, 서버의 최상단에서 실행시켰다. 5분마다 한번씩 콘솔에 로그를 출력하여 간단하게 확인해보고, 각 파라미터를 조정하여 개발서버에서 테스트를 진행 중이다 :)
- 특정 날짜를 넣어줄 수도 있고, cron과 다르게 객체로 변수를 선언하여 넣어줄 수도 있다. (*자세한 부분은 하단 링크 참조 ㅎㅎ* )

## 마치며
- 쇼핑몰 개발 당시에도, 스케줄러를 사용하여 주문에 관련하여 메일이나 문자를 보내는 로직을 스케줄러를 이용해서 개발해보고 싶었는데 당시에 배포가 급해서 미뤄뒀었다.
- 그리고 지금 업무에서 하게 됐는데, 테스트를 마친 후 결과를 보고 실서비스에도 적용될 예정이다.
- 이번에 각 모듈을 비교하며 개발자가 한 기능을 하는 모듈을 사용하고 싶을 때 유사한 모듈들의 정보를 끌어와 비교해주는 웹서비스를 만들어볼까? 하고 문득 생각하게 되었다 :)

## References
- [npm_node-schedule]

[npm_node-schedule]: https://www.npmjs.com/package/node-schedule
