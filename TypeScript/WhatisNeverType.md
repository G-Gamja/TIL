# Never 타입에 대해서
https://ui.toast.com/weekly-pick/ko_20220323

타입은 가능한 값의 집합이다. 예를 들어, string 타입은 무한히 가능한 모든 문자열의 집합을 의미한다. 그래서 변수의 타입을 string으로 지정하면, 해당 변수는 오직 가능한 집합 내의 값만을 가질 수 있다.
```typescript
let foo: string = 'foo'
foo = 3 // ❌ 숫자는 문자열 집합에 속하지 않는다.
```
타입스크립트에서 never 타입은 값의 공집합이다. 사실 또 다른 인기 자바스크립트 타입 시스템인 Flow에서 never 타입은 empty 타입과 같다.

따라서 집합에 어떤 값도 없기 때문에, never 타입은 any 타입의 값을 포함해 어떤 값도 가질 수 없다. 그래서 never 타입은 때때로 점유할 수 없는 또는 바닥 타입이라고 불린다

```typescript
declare const any: any
const never: never = any // ❌ 'any' 타입은 'never'타입에 할당할 수 없다.
```

## never가 필요한 이유

숫자 체계에 아무것도 없는 양을 나타내는 0처럼 문자 체계에도 불가능을 나타내는 타입이 필요하다.

"불가능"이라는 단어 자체는 모호하다. 타입스크립트에서는 "불가능"을 아래와 같이 다양한 방법으로 나타내고 있다.

* 값을 포함할 수 없는 빈 타입  
    * 제네릭과 함수에서 허용되지 않는 매개변수
    * 호환되지 않는 타입들의 교차 타입
    * 빈 합집합(무의 합집합)
* 실행이 끝날 때 호출자에게 제어를 반환하지 않는 함수의 반환 타입
    * 예) Node의 process.exit
    * void는 호출자에게 함수가 유용한 것을 반환하지 않는다는 것이므로 혼동하지 않도록 한다.
* 절대로 도달할수 없을 esle 분기의 조건 타입
  

## never의 동작 방식

숫자에 0을 더하면 그대로 그 숫자나 나오는 것 과 비슷한 로직
```typescript
type Res = never | string // string

type Res = never & string // never
```

## never 사용 예
