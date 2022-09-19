## 함수에서 중요한 타입들
| 타입| predicate |
|---|:---:|
| `void` | 	리턴 타입으로 사용하기 위해 의도된 undefined 의 서브타입. | 
| `T[]` | 수정가능한 배열들, 또한 Array<T> 으로 사용가능 |  
| `[T, T]` | typeof b === "boolean" |  
| `undefined` | 고정된 길이지만 수정 가능한 튜플|  
| `(t: T) => U` |함수 |  

함수 구문에는 매개변수 이름이 포함되어 있습니다. 익숙해지기 꽤 어렵습니다!
```typescript
let fst: (a: any, d: any) => any = (a, d) => a;
// 또는 좀 더 정확하게 말하자면:
let snd: <T, U>(a: T, d: U) => U = (a, d) => d;
```
```typescript
let o: { n: number; xs: object[] } = { n: 1, xs: [] };
```

함수 레퍼런스를 전달
onClick={submitAccountName}
와 onClick={submitAccountName()}
의 차이점

첫번쨰는 함수를 실행이 아니라 그냥 전달이고
아래는 실행이라 온클릭시에 실행이 아니고 그냥 바로 실행시킨다는 의미
괄호를 달면 실행을 시킨다는 의미이다
