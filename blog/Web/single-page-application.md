---
title: 'SPA (Single-Page-Application)'
category: 'Web'
desc: 'SPA(Single Page Application) 기본 개념'
date: '2021-07-15'
thumb: 'web'
---

# MPA (Multi-Page-Application)
- 일반적으로 우리가 웹사이트에 접속할 때, 서버에게 html파일에 대한 요청을 보낸다. 메인페이지의 경우 서버는 index.html을 전송하며 다른 페이지를 요청할 때 그 페이지에 맞는 html 파일을 전송한다.
- 과거에는 웹에서 제공되는 정보가 그리 많지 않았지만 지금은 정보가 무궁무진하게 많다.
- 이로 인해 페이지 전환마다 불필요한 로딩과 서버의 과부화 등의 단점이 발생하고 이를 해결하기 위해 SPA가 등장하였다.

# SPA(Single-Page-Application)
- 서버로부터 새로운 html파일을 불러오는 것이 아니라 현재의 페이지를 동적으로 다시 작성함으로써 단일 페이지 어플리케이션이라고 부른다.
- 메인페이지의 경우, vue라우터를 통하여 페이지를 렌더링하여 전송한다.  
 마찬가지로 다른 페이지를 요청하면 최상위 컴포넌트를 통해 라우터에 페이지를 그리고 그 페이지를 렌더링하여 전송한다.
- ***즉, index.html 단일 페이지만을 사용하며 하나의 완성된 구조의 html 파일이 아닌,  
현재 페이지(index.html)를 기준으로 동적으로 다시 작성하여 클라이언트와 소통하는 웹앱 또는 웹사이트인 것이다.***

## 라우팅(Routing)
- 더이상 페이지를 새로고침하지 않기 때문에 url에 일치하는 정보 또는 페이지를 보여주거나 변경된 페이지 이력 등을 브라우저가 히스토리(history api)로 인지하게 하는 방법이 필요하다.
- SPA에서 라우팅이란 url의 변화를 감지하고 사용자의 요청에 반응하는 것이다.
- 서버에 요청하지 않고 url만 변경하여 해시기호(#) 등을 통해 페이지 내부에서 찾게된다.

## 컴포넌트(Component)
- 하나의 블록으로 이해하는 것이 편할 것 같다. 이 블록들이 모여 한 페이지를 작성하고 특정 부분만 데이터를 바인딩하는 개념이다.

![component1.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Web/spa/1.png)

![component2.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Web/spa/2.png)

- 화면을 빠르게 구조화하여 일괄적인 패턴으로 개발할 수 있으며 재사용하기 훨씬 편리하며 유지보수비용이 적은 장점이 있다.
- 최상위 컴포넌트 위에 작성되는 것을 기준으로 트리 구조를 형성한다.

## 단점
- 웹 애플리케이션에 필요한 구성요소를 처음 한번에 가져오기에 초기 렌더링 속도가 상대적으로 느리다.
- 빈 html파일에 그림을 그리듯이 페이지를 보여주기 때문에, 검색엔진로봇이 크롤링할 때, 빈 화면을 가져오는 문제가 있다 (SEO issue)  
그러나 이는 Server Side Rendering 방식으로 해결이 가능하며, 나도 쇼핑몰을 개발할 때 이를 통해 검색엔진최적화 문제를 해결하였다.
  
  &nbsp;
### References
> - <https://www.kdata.or.kr/info/info_04_view.html?field=&keyword=&type=techreport&page=43&dbnum=174725&mode=detail&type=techreport>
