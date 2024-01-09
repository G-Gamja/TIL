## Controller란?

NestJS의 Controller는 애플리케이션의 엔드포인트를 정의하고 해당 엔드포인트에 대한 동작을 구현하는 주체입니다. 클라이언트와의 상호 작용 및 HTTP 요청에 대한 응답을 담당하며, 애플리케이션의 핵심 로직이 여기에 위치합니다.

## 기본 구조

NestJS에서 기본적인 Controller는 다음과 같이 작성됩니다.

```typescript
import { Controller, Get } from "@nestjs/common";

@Controller("example")
export class ExampleController {
  @Get()
  getExample(): string {
    return "Hello, NestJS!";
  }
}
```

- `@Controller('example')`: 해당 컨트롤러의 기본 경로를 '/example'로 지정합니다.
- `@Get()`: HTTP GET 메서드에 대한 핸들러를 정의합니다.

## 핵심 기능

### 1. 라우팅

Controller는 라우터 역할을 수행하며, 각 메서드는 특정 엔드포인트에 매핑됩니다. 데코레이터를 사용하여 경로를 지정하고, 각 메서드에 해당하는 HTTP 메서드를 정의합니다.

```typescript
@Get(':id')
getExampleById(@Param('id') id: string): string {
  return `Hello, NestJS! Received ID: ${id}`;
}
```

### 2. 응답 처리

HTTP 응답을 생성하고 반환하는 데 사용됩니다. 다양한 응답 형식 및 상태 코드를 처리할 수 있습니다.

```typescript
@Get()
getExample(): { message: string } {
  return { message: 'Hello, NestJS!' };
}
```

### 3. 의존성 주입

NestJS는 의존성 주입을 지원하며, Controller에서 서비스나 다른 컴포넌트를 주입하여 사용할 수 있습니다.

```typescript
constructor(private readonly exampleService: ExampleService) {}

@Get()
getExample(): string {
  return this.exampleService.getData();
}
```

### 4. 미들웨어

Controller에 미들웨어를 적용하여 특정 엔드포인트에 대한 요청 또는 응답을 가로챌 수 있습니다.

```typescript
@UseMiddleware(LoggerMiddleware)
@Get()
getExample(): string {
  return 'Hello, NestJS!';
}
```

## 마무리

NestJS의 Controller는 애플리케이션의 핵심 부분 중 하나로, 라우팅, 응답 처리, 의존성 주입 등 다양한 기능을 담당합니다. 데코레이터를 통한 간결한 구문과 함께 모듈화된 개발을 지원하여 유지보수성을 향상시키고, 효과적인 웹 애플리케이션 개발을 가능케 합니다. 자세한 내용은 [NestJS 공식 문서](https://docs.nestjs.com/)를 참조하세요.

```

```
