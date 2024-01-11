Nest Js에서 모듈은 기본적으로 싱글턴(Singleton)이기 때문에 여러 모듈 간에 쉽게 providers의 동일한 인스턴스를 공유할 수 있습니다. 때문에 모든 모듈은 공유 모듈(Shared modules)이 됩니다. 일단 생성되면 모든 모듈에서 재사용할 수 있습니다.

예를 들어 다른 모듈에서 study service 인스턴스를 공유하며 사용하고 싶을 때, 아래와 같이 exports에 study service (providers)를 추가하여 내보내기를 해야 합니다. exports 또한 @Module() 데코레이터의 providers 속성처럼 여러 개의 providers를 추가할 수 있습니다.

```typescript
// study.module.ts

@Module({
  controllers: [StudyController],
  providers: [StudyService, ChildService, TestServiceA],
  exports: [StudyService],
})
export class StudyModule {}
```

이렇게 exports로 내보낸 providers는 다른 모듈에서 imports 해서 사용할 수 있으며 동일한 인스턴스를 공유합니다.

예를 들어 아래처럼 sub.module.ts에서 StudyModule을 imports 함으로써 StudyService의 인스턴스에 접근할 수 있게 됩니다. StudyService에는 createStudy, findAll 메소드가 존재하는데 StudyService를 impots 함으로서 SubController에서는 StudyService의 createStudy와 finAll 메소드를 사용할 수 있게 됩니다.

```typescript
// sub.module.ts

@Module({
  imports: [StudyModule],
  controllers: [SubController],
})
export class SubModule {}
```

만약 SubModule에서 imports 할 때 StudyModule이 아니라 `StudyService`라고 작성하면 어떻게 될까요? 언뜻 보면 StudyModule에서 exports 한 것은 StudyService이기 때문에 괜찮을 것처럼 보입니다. 하지만 아래와 같이 Nest can't resolve dependencies of the SubController 오류가 발생합니다. imports는 반드시 모듈 형태로 가져와야 하는데 그렇지 않았기 때문에 발생하는 오류입니다. 따라서 StudyService가 아니라 StudyModule를 imports 받아야 합니다.

```ts
// sub.module.ts
// ERROR!
@Module({
  imports: [StudyService],
  controllers: [SubController],
})
export class SubModule {}
```

# import하는 이유

Nest Js의 모듈은 자신의 범위 내에서 providers를 캡슐화하기 때문에 어떤 모듈에서 다른 모듈의 provider(Service로직)를 사용하기 위해서는 해당 모듈 클래스를 imports 해야 합니다.

## 중요

반드시 어떤 기능을 다른 모듈에서 사용하게 만들려면 exports는 꼭 작성해주셔야 합니다.
