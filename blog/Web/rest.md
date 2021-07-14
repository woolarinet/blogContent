---
title: 'REST, REST API, RESTful API'
category: 'Web'
desc: 'REST에 관한 전반적인 개념 정리'
date: '2021-07-15'
thumb: 'web'
---

# REST (Representational State Transfer)

![rest.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Web/rest/1.png)
- HTTP URI를 통해 자원을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미한다.
- 클라이언트와 서버사이의 통신 방식 중 하나이며 모든 것을 Resource(명사)로 표현하며 세부 Resource에는 id를 붙인다.
- HTTP 표준을 따르는 모든 플랫폼에서 사용이 가능하며 별도의 인프라를 구축할 필요가 없다.
- Server-Client 구조를 가지며 stateless 특성을 가진다. 
- JSON 혹은 XML 메시지 형태로 데이터를 주고 받는다는데 나는 개발 시 대부분 JSON으로 주고받았다.
- Resource Oriented Architecture이다. (*Resource 지향 아키텍처* )
- 계층화가 되어있다. Client는 REST API 서버만 호출하고 REST API는 다중 계층으로 구성이 가능하다.

  &nbsp;
# REST API

- REST 기반인 서비스 API를 뜻하며, 최근 Open API를 제공하는 대부분의 회사는 REST API를 제공한다.

## 설계 규칙

 - URI는 정보의 자원을 표현해야 한다.
 - 자원에 대한 행위는 HTTP Method로 표현한다.
 - 슬래시 구분자(/)는 계층 관계를 나타내야 한다.
 - URI마지막 문자로 슬래시(/)를 포함하지 않는다.
 - 하이픈(-)은 URI가독성을 높이는데 사용   *ex) 긴 URI*
 - 밑줄(_)은 URI에 사용하지 않는다.
 - URI 경로에는 소문자가 적합하다.
 - 파일확장자는 URI에 포함하지 않는다.
 - Resource 간에는 연관 관계가 있어야 한다.

  &nbsp;
# RESTful API

- 이해하고 사용하기 쉬운 REST API를 만드는 것을 목적으로 하며 'REST API'를 제공하는 웹서비스를 'RESTful'하다고 할 수 있다.

## RESTful 하지 못 한 경우

- ex1) CRUD 기능을 모두 POST로만 처리
- ex2) route에 resource, id 외의 정보가 들어가는 경우

  &nbsp;
# 마치며(?)

##### 나는 개발 시에 거의 REST API를 사용한다. 파이썬과 js를 번갈아가며 코딩을 하는데 언어에 규약 없이 통신을 주고받을 수 있어서 참 편한 것 같다.
##### 선호뮤직의 서버단도 대부분이 REST를 기반으로 하였는데 설계규칙을 다시 공부해보니 지키지 못한 부분이 두개 보인다.. 그래도 항상 당시 공부하며 개발했던 부분이 참 많이 부족했다고 느꼈는데 최근들어 복습을 하면서 아 그래도 내가 꽤 괜찮게 개발하고 있었구나 느끼는 부분도 많다ㅎ 
##### pg사 대행업체의 REST API를 사용하며 통신 당시, 데이터 포맷때문에 그곳 개발자랑 얘기를 나눴던 기억도 난다. 처음에 내가 틀리다고 그랬었는데 결국 내가 맞아서 그 회사의 API를 고쳤었는데 기분이 좋았다.
##### 요즘은 graphql을 공부하며 개발하고 있는데 블로그도 graphql 기반으로 개발 중이다. 이 부분도 나중에 정리해봐야겠다.

### References
> - <https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html>
> - <https://gyoogle.dev/blog/web-knowledge/REST%20API.html>
> - <https://blog.naver.com/PostView.nhn?isHttpsRedirect=true&blogId=hoyeon0&logNo=50137172782>

