## DTO란?

DTO는 Data Transfer Object의 약자로, 데이터를 전송하거나 전달하기 위한 객체를 나타냅니다. 주로 네트워크를 통해 데이터를 주고받거나, 서비스 간의 데이터 교환 시에 사용됩니다. NestJS에서는 주로 API 엔드포인트의 요청과 응답을 처리하는 데에 DTO를 활용합니다.

## 기본 구조

DTO는 주로 클래스로 표현되며, 해당 클래스는 전달하고자 하는 데이터의 구조를 정의합니다. 예를 들어, 사용자 생성에 필요한 데이터를 담은 UserDTO는 다음과 같이 작성될 수 있습니다.

```typescript
export class UserDTO {
  readonly username: string;
  readonly email: string;
  readonly password: string;
}
```

## DTO 사용 이유

### 1. 데이터 검증

DTO를 사용하면 클라이언트로부터 전달된 데이터를 미리 정의된 구조로 검증할 수 있습니다. 이를 통해 불필요한 에러를 방지하고 안정적인 데이터 처리를 할 수 있습니다.

### 2. 코드 가독성과 유지보수성

DTO를 사용하면 API 엔드포인트에서 전달되는 데이터의 구조를 명확하게 정의할 수 있습니다. 이는 코드 가독성을 향상시키고, 유지보수가 더 쉬워집니다.

### 3. 레이어 간 데이터 전송

서비스 간 데이터를 주고받을 때 DTO를 사용하면 레이어 간의 인터페이스를 명확하게 정의할 수 있습니다. 이는 서비스 간의 통신을 단순화하고 오류를 최소화하는 데 도움이 됩니다.

## DTO의 예시

NestJS에서 Controller에서 DTO를 사용하는 예시를 살펴보겠습니다.

```typescript
import { Controller, Post, Body } from "@nestjs/common";
import { UserDTO } from "./user.dto";

@Controller("users")
export class UsersController {
  @Post()
  createUser(@Body() userDTO: UserDTO): string {
    // userDTO의 데이터를 이용한 유저 생성 로직
    return `User ${userDTO.username} created successfully!`;
  }
}
```

위의 코드에서 `@Body()` 데코레이터를 통해 클라이언트로부터 전달된 데이터를 `UserDTO`로 타입 정의하여 받아옵니다.

### 왜 타입스크립트의 인터페이스를 사용하지 않나요?

타입스크립트의 인터페이스는 컴파일 시점에만 존재하며, 런타임에서는 사라집니다. 따라서 인터페이스는 데이터의 구조를 정의하는 데에는 적합하지만, 데이터의 전송을 위한 DTO로는 적합하지 않습니다. DTO는 런타임에서도 존재해야 하며, 데이터의 전송을 위한 목적으로 사용되기 때문입니다.

## 마무리

DTO는 NestJS에서 데이터를 전송하고 전달하기 위한 중요한 도구 중 하나입니다. 데이터의 구조를 미리 정의하고 검증함으로써 코드의 안정성과 가독성을 향상시키며, 레이어 간의 통신을 간소화합니다. 더 많은 정보는 [NestJS 공식 문서](https://docs.nestjs.com/)에서 확인할 수 있습니다.
