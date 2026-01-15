[number] (Indexed Access Type)
이 부분이 가장 핵심적인 테크닉입니다.

배열 타입 뒤에 [number]를 붙이는 것은 **"이 배열의 숫자 인덱스(0, 1, 2...)로 접근했을 때 나올 수 있는 모든 값의 타입이 무엇인가?"**를 묻는 것과 같습니다.

aa 배열의 모든 요소는 서로 다른 리터럴 문자열들이므로, 이들의 유니온(Union) 타입이 생성됩니다.

최종 결과: InitialKeys
결과적으로 InitialKeys는 다음과 같은 타입을 가지게 됩니다.

```typescript
const aa = ["paramsV11", "assetsV11", "userCurrencyPreference", "userPriceTrendPreference"] as const;

type InitialKeys = (typeof aa)[number];

type InitialExtensionStorage = Pick<ExtensionStorage, InitialKeys>;
```

```ts

type InitialKeys =
  | 'paramsV11'
  | 'assetsV11'
  | 'userCurrencyPreference'
  | ...
  | 'userPriceTrendPreference';
```
