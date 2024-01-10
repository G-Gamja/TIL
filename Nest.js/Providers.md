providers를 사용하여 서비스를 등록하면, 해당 서비스는 NestJS의 의존성 주입 시스템에 의해 관리됩니다. 이로 인해 해당 서비스는 생성자를 통해 자동으로 주입받을 수 있으며, 싱글턴 패턴을 따르게 됩니다. 즉, 애플리케이션 내에서 한 번만 생성되고, 같은 인스턴스가 여러 곳에서 재사용됩니다.

반면, 함수를 직접 export하고 import하여 사용하는 경우, 이러한 의존성 주입 기능을 사용할 수 없습니다. 함수는 호출될 때마다 새로 실행되며, 상태를 유지하지 않습니다. 또한, 함수를 사용하는 측에서는 해당 함수의 의존성을 직접 관리해야 합니다.

따라서, 서비스의 상태를 유지하거나, 서비스 간의 의존성을 자동으로 관리하려면 providers를 사용하여 서비스를 등록하는 것이 좋습니다.

? 의존성 주입 기능이란 뭐지?
-> 같은 인스턴스가 여러 곳(값을 유지하면서)에서 재사용된다

# 커스텀 프로바이더

NestJS에서 `@Injectable()` 데코레이터가 적용된 클래스를 "프로바이더(Provider)"라고 합니다. 프로바이더는 주로 의존성 주입(Dependency Injection)을 통해 컨트롤러, 서비스, 미들웨어 등에서 사용되는 인스턴스화된 객체를 나타냅니다. 프로바이더는 애플리케이션 전반에서 싱글톤으로 존재하며, 필요한 곳에서 주입되어 사용됩니다.

프로바이더를 생성하는 몇 가지 기본적인 방법은 다음과 같습니다:

1. **클래스로 정의된 서비스 프로바이더:**

   ```typescript
   // 예시: UserService 클래스
   @Injectable()
   export class UserService {
     findAll(): string {
       return "This action returns all users";
     }
   }
   ```

2. **팩토리 프로바이더:**

   ```typescript
   // 예시: FactoryProvider 사용
   export const userServiceFactory = {
     provide: "USER_SERVICE",
     useFactory: () => {
       return new UserService();
     },
   };

   // 모듈에서 프로바이더 등록
   @Module({
     providers: [userServiceFactory],
   })
   export class AppModule {}
   ```

3. **값 프로바이더:**

   ```typescript
   // 예시: ValueProvider 사용
   export const configValue = {
     provide: "CONFIG",
     useValue: { maxUsers: 10, appName: "NestJS App" },
   };

   // 모듈에서 프로바이더 등록
   @Module({
     providers: [configValue],
   })
   export class AppModule {}
   ```

4. **클래스의 프로바이더에 의존성 주입:**

   ```typescript
   // 예시: 의존성 주입을 통한 클래스 프로바이더
   @Injectable()
   export class AppService {
     constructor(private readonly userService: UserService) {}

     getAppInfo(): string {
       const usersInfo = this.userService.findAll();
       return `App Information: ${usersInfo}`;
     }
   }
   ```

프로바이더는 NestJS 애플리케이션의 모듈에서 등록되어야 합니다. 모듈에서 `providers` 배열에 프로바이더를 추가함으로써 NestJS는 이를 식별하고 의존성 주입을 관리합니다.
