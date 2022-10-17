# == 와 === 의 차이점
https://codechacha.com/ko/javascript-compare-strings/

# && 연산자

왼쪽의 피연산자가 True(참)인 경우, 오른쪽 피연산자 값을 반환한다.
```typescript
  const val: string = `VALUE TEST`;
  const isExist: boolean = true;
  console.log(`${isExist && val}`);

  // [Result]
    VALUE TEST
```

왼쪽의 피연산자가 False(거짓)인 경우, 왼쪽 피연산자 값을 반환한다.

```typescript
  const val: string = `VALUE TEST`;
  const isExist: boolean = false;
  console.log(`${isExist && val}`);
  // [Result]
    false
```

# || 연산자

왼쪽의 피연산자가 True(참)인 경우, 왼쪽 피연산자 값을 반환한다.
```
  const val: string = `VALUE TEST`;
  const isExist: boolean = true;
  console.log(`${isExist || val}`);
  [Result]
    true
```
왼쪽의 피연산자가 False(거짓)인 경우, 오른쪽 피연산자 값을 반환한다.

```
  const val: string = `VALUE TEST`;
  const isExist: boolean = false;
  console.log(`${isExist || val}`);
  [Result]
    VALUE TEST
```

* False에 해당하는 데이터
    *  숫자: 0
    *  문자열: 빈 문자열
    *   객체: null, undefined