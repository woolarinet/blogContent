---
title: '웹 어플리케이션 아키텍쳐란?'
category: 'Web'
desc: '웹 어플리케이션 아키텍쳐(Web Application Architecture)의 기본적인 이해'
date: '2021-07-14'
thumb: 'web'
---

# 웹 어플리케이션 아키텍쳐 (Web Application Architecture)

![web_app_architecture.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Web/Architecture/1.png)

웹 어플리케이션의 구조를 간략히 봤을 때 위와 같은 구조가 나온다. 크게는 클라이언트 사이드(프레젠테이션 로직 처리)과 서버 사이드(비지니스 로직 처리)로 나뉘며 기본적으로 웹 브라우저, 웹 서버, 웹 어플리케이션 서버 데이터베이스 서버로 구성되어 있다.
  
&nbsp;    
## 웹 브라우저 (Web Browser)

웹 브라우저는 서버와의 Request, Response 을 통해 상호작용하며 어플리케이션의 기능을 처리하고 HTML과 CSS 명세에 따라 HTML파일을 해석 및 표시해준다.

![web_browser.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Web/Architecture/2.png)

웹 브라우저의 대략적인 구성요소이다.

**User Interface**는 사용자가 기능을 쉽게 사용하도록 만든 UI요소로써 이전/다음, 북마크 등의 버튼을 의미하며

**Browser Engine**은 *UI*와 *Rendering Engine*사이의 동작을 제어한다.

**Rendering Engine**은 요청받은 컨텐츠를 표시한다. 각 웹 브라우저마다 엔진의 종류가 달라 사용하는 브라우저에 따라 일부 기능이 제한될 수 있다.

**Networking**은 네트워크 호출에 사용되며  
**JavaScript Interpreter**은 자바스크립트 코드를 파싱하고 실행하는 역할을 한다.

**UI Backend**는 기본적인 위젯을 그리며  
**Data Persistence**는 쿠키와 같은 자료들을 저장하는 계층이다.    
    
&nbsp;    
## 웹 서버 (Web Server)

클라이언트로부터 HTTP요청을 받아 정적 웹 페이지를 응답해주는 소프트웨어이다.

![web_server.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Web/Architecture/3.png)

##### 웹서버는 클라이언트의 요청을 HTTP를 기반으로하여 응답하는 기능을 담당하는데, 정적인 콘텐츠를 제공하며 *WAS(Web Application Server)*를 거치지 않고 바로 자원을 제공해준다. 또한, 동적인 컨텐츠 제공을 위해 클라이언트의 요청을 WAS로 보내 처리결과를 클라이언트에게 응답해준다.

*나는 선호뮤직을 개발할 때, NGINX를 이용하여 *Content Security Policy*를 설정해 보안을 한층 더 강화한 경험이 있다.*

&nbsp;    
## 웹 어플리케이션 서버 (Web Application Server)

  &nbsp;
![web_application_server.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Web/Architecture/4.png)

##### **Web Application Server**란 동적컨텐츠를 제공하기 위한 *Application Server*이다. 주로 데이터베이스 서버와 같이 수행되며, 트랜잭션 처리, 보안, 비지니스 로직 처리 등을 담당한다.
*Web Server*와 *Web Container*의 기능을 모두 갖춘 서버라고 할 수 있을 것 같다.

&nbsp;    
### References
> - <http://clipsoft.co.kr/wp/blog/%EC%9B%B9-%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98-%EC%95%84%ED%82%A4%ED%85%8D%EC%B3%90-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90/>
> - <https://goldsony.tistory.com/37>