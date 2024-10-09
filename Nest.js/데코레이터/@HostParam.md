# @HostParam()

`@HostParam()` 데코레이터는 NestJS에서 사용되는 데코레이터 중 하나로, HTTP 요청의 호스트 부분에서 파라미터를 추출하는 데 사용됩니다. 이 데코레이터는 주로 도메인 파라미터를 추출하는 데에 활용됩니다.

HTTP 요청의 URL에서 호스트 부분은 주로 도메인을 의미하며, 이를 통해 도메인의 특정 값을 컨트롤러나 프로바이더에서 사용할 수 있습니다.

다음은 `@HostParam()` 데코레이터의 간단한 사용 예제입니다:

```typescript
import { Controller, Get, HostParam } from "@nestjs/common";

@Controller("users/:id")
export class UsersController {
  @Get()
  getUser(@HostParam("id") id: string) {
    // 호스트 파라미터(id)를 사용하여 로직을 수행
    return `User ID: ${id}`;
  }
}
```

이 예제에서는 `@Controller` 데코레이터를 사용하여 `users/:id` 엔드포인트를 정의하고, `@Get` 데코레이터를 사용하여 GET 요청을 처리합니다. `@HostParam('id')` 데코레이터를 사용하여 호스트 파라미터를 추출하고 해당 값을 컨트롤러의 `getUser` 메서드에 전달합니다.

실제 요청이 오면, `/users/123`와 같은 URL에서 `123`이라는 호스트 파라미터를 추출하여 사용할 수 있습니다. 이러한 방식으로 호스트 파라미터를 추출하면 도메인과 관련된 데이터를 동적으로 처리할 수 있습니다.
