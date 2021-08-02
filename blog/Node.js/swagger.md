---
title: 'Swagger로 api 문서 작성하기'
category: 'Node.js'
desc: 'Swagger 사용법 :)'
date: '2021-08-02'
thumb: 'node'
---

##### *자세한 부분은 하단의 References를 참고해주세요~!*

# Swagger
- API 제작이 끝난 후, 프론트 개발자 분에게 API를 전달할 때 github, notion 등 많은 방법이 있지만, 수기로 작성해야되는 불편함이 있다.
- Swagger 공식문서를 보면 다음과 같이 말한다.
  
  >  Accurate, up-to-date documentation is essential to a successful API initiative. With SwaggerHub, you can generate interactive documentation automatically during design, making it easy for both API consumers and internal users to learn and work with your APIs.
  >
  >  Enhance your approach to API documentation by: 
  >  - Providing a blueprint for API behavior to inform your development
  >  - Importing and hosting OAS definitions in one central platform 
  >  - Managing access to API docs with built-in permissions and user roles 
  >  - Versioning and publishing OAS documentation to ensure consistency

- 즉, api의 개발과 동시에 명세방법에 따라 주석을 작성하여 api문서의 자동화와 최신화를 동시에 하여 사용자는 최소한의 구현논리로 서비스를 이해하고 상호 작용할 수 있다.

![swagger.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Node.js/swagger/1.png)
- 위 화면과 같이 프로젝트 내에서 정의한 내용들을 보기좋은 UI로 제공한다 :)

## Installation

``` javascript
npm i swagger-jsdoc swagger-ui-express --save-dev
```
- 개발용 라이브러리이기 때문에 devDependencies에 추가해준당

## Options

``` javascript
// ../config/swagger.js

const swaggerUi = require('swagger-ui-express');
const swaggereJsdoc = require('swagger-jsdoc');

const options = {
    swaggerDefinition: {
        info: {
            title: 'Test API',
            version: '1.0.0',
            description: 'Test API with express',
        },
        host: 'localhost:3000',
        basePath: '/'
    },
    apis: ['./routes/*.js', './routes/api/*.js']
};

const specs = swaggereJsdoc(options);

module.exports = {
    swaggerUi,
    specs
};
```
- 프로젝트 구조와 파일 형식에 맞게 config 파일을 만들어준다.
- info 객체에는 title, description, contact 정보, version 명시할 수 있다.

## Route Setting

``` javascript
const { swaggerUi, specs } = require('/config/swagger');

//...

app.use('/api', swaggerUi.serve, swaggerUi.setup(specs))
```

## Use

``` javascript
/**
 * @swagger
 *  /main/community:
 *    get:
 *      tags:
 *      - main
 *      summary: 커뮤니티 콘텐츠 목록
 *      description: 좋아요를 가장 많이 받은 커뮤니티 글 Top 20
 *      produces:
 *      - application/json
 *      parameters:
 *        - in: query
 *          name: category
 *          required: false
 *          schema:
 *            type: integer
 *            description: 카테고리
 *      responses:
 *       200:
 *        description: 커뮤니티 조회 성공
 */
```
- 각 api의 http 메소드, 파라미터 조건 등에 맞게 작성하여준다.
- 그 후, 배포되어있는 개발서버에 반영해주면 사진과 같이 명세서가 자동으로 업데이트 된다. (*위 코드와는 다른 예제 명세서이다.* )
  ![swagger2.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Node.js/swagger/2.png)

### 마치며
- 오늘 UI의 특정 부분을 다른 방식으로 교체하는 작업을 맡았는데 프론트 개발자분과 협업하며 swagger를 사용하였고, 집에와서 다시 정리를 해보았다.
- 점점 더 생산성을 높이기 위해 단위테스트 모듈 및 여러 자동화 도구를 공부하며 실무에서 사용해볼 예정이다 :)
- 하루하루 폭풍성장을 해버릴 예정 ㅎㅎ

## References
- [Swagger-Docs]
- [Swagger-github]
- <https://gngsn.tistory.com/69>

[Swagger-Docs]: https://swagger.io/docs/

[Swagger-github]: https://github.com/OAI/OpenAPI-Specification/blob/main/versions/3.0.3.md#infoObject