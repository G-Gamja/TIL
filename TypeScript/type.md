# 타입지정하기
```typescript
let a: string = 'text'; // 문자열
let b: number = 0; // 숫자형
let c: boolean = true; // 논리형
let d: any = true; // 어떤 타입이 올지 모를 때
let e: string | number = '0'; // 문자열이나 숫자가 올 때
```
## 문자열
```typescript
let str01: string = 'text';
let str02: string = `my name is ${val}`;
```

## 배열
```typescript
// 문자열만 가지는 배열
let arr01: string[] = ['a', 'b', 'c'];
let arr02: Array<string> = ['a', 'b', 'c'];

// 숫자형만 가지는 배열
let arr03: number[] = [1, 2, 3];
let arr04: Array<number> = [1, 2, 3];

// Union 타입(다중 타입)의 문자열과 숫자를 동시에 가지는 배열
let arr05: (string | number)[] = [1, 'a', 2, 'b', 'c', 3];
let arr06: Array<string | number> = [1, 'a', 2, 'b', 'c', 3];

// 배열이 가지는 값의 타입을 추측할 수 없을 때 any를 사용할 수 있다.
let arr07: (any)[] = [1, 'a', 2, 'b', 'c', 3];
let arr08: Array<any> = [1, 'a', 2, 'b', 'c', 3];

let arr07: (any)[] = [1, 'a', 2, 'b', 'c', 3];
let arr08: Array<any> = [1, 'a', 2, 'b', 'c', 3];
```

## 함수
```typescript
function sum(a: number, b: number): number {
  return a + b;
  return null; => return 타입이 number가 아니어서 에러
}
console.log(sum(2, 3)); // 5
```
## 객체

```typescript
기본
let obj: object = {};

좀더 구체화해서
let user: { name: string, age: number } = {
  name: 'a',
  age: 20
};
console.log(user); // {name: "a", age: 20}

아래와 상동함
type User =  { name: string, age: number };
let user: User = {
  name: 'a',
  age: 20
};
console.log(user); 

```

## type정의하기

### 유니언 방식
유니언은 타입이 여러 타입 중 하나일 수 있음을 선언하는 방법입니다. 예를 들어, `boolean` 타입을 `true` 또는 `false로` 설명할 수 있습니다:

```typescript
type MyBool = true | false;
```
이러한 방식은 리터럴집합에 흔히 사용된다.
```typescript
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
type OddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
```
```typescript
array 또는 string을 받는 함수
function getLength(obj: string | string[]) {
  return obj.length;
}
```
### 타입 검사
| 타입| predicate |
|---|:---:|
| `string` | typeof s === "string" | 
| `number` | typeof n === "number" |  
| `boolean` | typeof b === "boolean" |  
| `undefined` | typeof undefined === "undefined" |  
| `function` |typeof f === "function" |  
| `array` | Array.isArray(a) |  

## 제네릭
제네릭은 `타입`에 `변수`를 제공하는 방법입니다.

배열이 일반적인 예시이며, 제네릭이 없는 배열은 어떤 것이든 포함할 수 있습니다. 제네릭이 있는 배열은 배열 안의 값을 설명할 수 있습니다.

```typescript
type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectWithNameArray = Array<{ name: string }>;
```

```typescript
// @errors: 2345  Type자리에 뭐로 선언되느냐에 따라 다르게 바뀌겠죠?
interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}

// 이 줄은 TypeScript에 `backpack`이라는 상수가 있음을 알리는 지름길이며
// const backpack: Backpack<string>이 어디서 왔는지 걱정할 필요가 없습니다.
declare const backpack: Backpack<string>;

// 위에서 Backpack의 변수 부분으로 선언해서, object는 string입니다.
const object = backpack.get();

// backpack 변수가 string이므로, add 함수에 number를 전달할 수 없습니다.
backpack.add(23);
```