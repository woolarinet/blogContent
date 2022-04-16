---
title: 'NestJS + Swagger api 명세 작성'
category: 'Node.js'
desc: 'NestJS에 Swagger 적용하기'
date: '2022-04-15'
thumb: 'node'
keyword: 'Node.js', 'swagger', 'open api', 'NestJS'
---

##### *자세한 부분은 하단의 References를 참고해주세요*

# Swagger
- Swagger 공식문서를 보면 다음과 같이 말한다.
  
  >  Accurate, up-to-date documentation is essential to a successful API initiative. With SwaggerHub, you can generate interactive documentation automatically during design, making it easy for both API consumers and internal users to learn and work with your APIs.
  >
  >  Enhance your approach to API documentation by: 
  >  - Providing a blueprint for API behavior to inform your development
  >  - Importing and hosting OAS definitions in one central platform 
  >  - Managing access to API docs with built-in permissions and user roles 
  >  - Versioning and publishing OAS documentation to ensure consistency

- 즉, api의 개발과 동시에 명세방법에 따라 주석을 작성하여 api문서의 자동화와 최신화를 동시에 하여 사용자는 최소한의 구현논리로 서비스를 이해하고 상호 작용할 수 있다고 판단된다.

## Installation

```bash
npm install --save @nestjs/swagger swagger-ui-express
```

## Options

```typescript
// swagger.option.ts

import { DocumentBuilder } from '@nestjs/swagger';

export class SwaggerFactory {
  public builder: DocumentBuilder;
  constructor (private readonly app: INestApplication) {
    this.builder = new DocumentBuilder();
  }

  public setup() {
    const document = this.createDocument();
    SwaggerModule.setup('api-docs', this.app, document);
  }
  
  private initializeOptions() {
    return this.builder
      .setTitle('My Server')
      .setDescription('This is API Docs for My Server')
      .setVersion('1.0')
      .build();
  }

  private createDocument() {
    const config = this.initializeOptions();
    return SwaggerModule.createDocument(this.app, config);
  }
}
```

## Setting

```typescript
// main.ts

...

const swaggerFactory = new SwaggerFactory(app);
swaggerFactory.setup();

...
```

## Use
- 컨트롤러 최상단에 공통적인 Response에 대한 정의를 한다.
- 각 컨트롤러 위에는 요청과 응답에 대한 상세정의 및 Http 상태코드 등을 정의한다.
```typescript
// app.controller.ts

import {
  ApiBadRequestResponse,
  ApiBody, ApiCreatedResponse,
  ApiInternalServerErrorResponse,
  ApiNoContentResponse,
  ApiNotFoundResponse,
  ApiOkResponse,
  ApiOperation, 
  ApiQuery,
  ApiTags
} from '@nestjs/swagger';

@Controller('Example')
@ApiTags('Example')
@ApiBadRequestResponse({
  status: 400,
  description: '잘못된 파라미터'
})
@ApiInternalServerErrorResponse({
  status: 500,
  description: '서버 로직 문제'
})
export class ExampleController {
  
  // ...

  @Post()
  @HttpCode(201)
  @ApiOperation({
    summary: 'create',
    description: 'This is a description for POST API'
  })
  @ApiBody({ type: CreateExampleDto })
  @ApiCreatedResponse({ description: 'This is a Response', type: String })
  public async create(@Body() exampleData: CreateExampleDto) {
    return await this.exampleService.create(categoryData);
  }

  // ...
}
```
- 그리고 interface 부분이나 Entity 부분에도 각 Property에 대한 정의를 해준다.
```typescript
// create-example.dto.ts

import { IsString, IsNumber, IsOptional } from "class-validator";
import { ApiProperty, ApiPropertyOptional } from '@nestjs/swagger';

export class CreateExampleDto {
  @IsNumber()
  @IsOptional()
  @ApiPropertyOptional({
    description: 'This is a Id Property',
    default: 0,
    example: 5
  })
  readonly id: number;

  @IsString()
  @ApiProperty({ description: 'This is a Name Property', required: true })
  readonly name: string
}
```
- 예시 페이지 (위 코드와 연관 없습니다.)
  ![swagger2.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Node.js/swagger/2.png)

## References
- [Swagger-Docs]
- [Swagger-github]
- [NestJS-Docs]
 
[Swagger-Docs]: https://swagger.io/docs/

[Swagger-github]: https://github.com/OAI/OpenAPI-Specification/blob/main/versions/3.0.3.md#infoObject

[NestJS-Docs]: https://docs.nestjs.com/openapi/introduction