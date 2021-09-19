---
title: 'Serverless Architecture란?'
category: 'Devops'
desc: 'Serverless Architecture 간단 정리'
date: '2021-08-13'
thumb: 'devops'
keyword: 'Devops', 'Serverless', 'Lambda', 'AWS', 'Architecture'
---

# Serverless Architecture
- 서버리스 아키텍쳐란 따로 서버를 관리할 필요없이 어플리케이션을 빌드하거나 실행할 수 있는 방법이라고 설명한다.
- 개발자는 따로 서버를 유지보수하거나 스케일링 등의 인프라 관리 작업을 할 필요 없이 온전히 서비스 기능 구현에 집중할 수 있다.
- 여기에서 서버리스란, 서버가 없다는 뜻이 아니라 별도의 인프라 관리작업을 할 의무를 가지지 않고 서비스를 운영할 수 있다는 의미라고 한다.

## Features
### Event-based
- 서버리스는 이벤트기반 시스템을 사용한다. 따라서 이벤트가 비동기식으로 생성되며 응답을 기다리지 않고 이벤트가 발생할 때 실행할 수 있다.
- 느슨한 결합을 사용하면, 서비스가 실패해도 다른 서비스는 영향을 받지 않는다.
- 필요한 만큼 사용할 수 있으며 **이벤트 제작자** , **이벤트 라우터** , **이벤트 소비자** , 세가지 구성요소가 독립적으로 배포, 업데이트, 확장된다.
### Decomposing drives better observability
- 어플리케이션을 작고 또 작은 함수로 쪼개다 보면 뛰어난 관찰력을 얻을 수 있다.
- 이는 확장성을 높여주고 유지보수비용을 줄여준다!
### Reducing architecture costs
- 서버를 올리는 것만으로 비용이 청구되지 않고, 요청 수와 이벤트 작업량에 따른 과금이 발생한다.
- 또한 서버관리 리소스가 감축되고 서버 스펙을 유연하게 조정이 가능하여 여러 관점에서의 비용이 감소된다
### Focusing more on UX
- 아키텍처를 고려하는 비용만큼 UX에 더 집중할 수 있게 된다.
- 이는 사용자로 하여금 서비스에 대한 신뢰도와 만족감을 높여줄 수 있다.

## XaaS (X as a Service)
- 우리가 사용자에게 제공하는 클라우드 서비스들에는 몇가지 종류가 있다. 
### IaaS (Infrastructure as a Service)
- 필요한 만큼의 서버, 스토리지 등의 인프라 자원을 사용하고 지불하는 서비스 (AWS EC2)
### PaaS (Platform as a Service)
- 특정 서비스를 쉽게 개바해주는 개발 플랫폼 제공 서비스 (AWS Elastic Beanstalk)
### BaaS (Backend as a Service)
- 백엔드의 기능을 일일히 개발하지 않고 API나 Plugin 형태로 제공하는 서비스 (Google Firebase)
### FaaS (Function as a Service)
- 우리가 흔히 얘기하는 서버리스.
- 이벤트에 반응하는 함수를 등록 후 해당 이벤트가 발생 시 함수가 실행 (AWS Lambda)
### SaaS (Software as a Service)
- 클라우드 환경에서 동작하는 응용프로그램을 제공하는 서비스
- 소프트웨어는 중앙에 호스팅 되고, 사용자는 브라우저 등의 클라이언트를 통해 접속 (Dropbox)
### CaaS (Containers as a Service)
- 클라우드나 데이터센터 등을 이용하여 컨테이너 기반 추상화를 통해 애플리케이션을 배포하고 관리하도록 지원하는 서비스 (AWS ECS)

## 마치며
- 회사가 점점 커져감에 따라, 추후를 대비하여 미리미리 서버를 재구축하는 것을 준비 중이다.
- 그 과정에서 최대한 효율을 살려 Serverless로 구현할 수 있는 부분은 그렇게 진행하기로 하였고 앞서 미리 가볍게 공부해보았다.
- 이제 깊은 부분까지 공부하여 구조는 어떻게 되었는지와 통신 원리를 이해해보려 한다.
- 완벽히 이해하여 실무에 적용하고 싶다ㅋ

## References
- [newrelic.com]
- [cloud.google.com]

[newrelic.com]: https://newrelic.com/blog/best-practices/what-is-serverless-architecture

[cloud.google.com]: https://cloud.google.com/eventarc/docs/event-driven-architectures?hl=ko