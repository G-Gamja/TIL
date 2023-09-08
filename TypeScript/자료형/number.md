# 값이 정수인지 소수인지 판별하는 법

소수를 parserInt안에 넣으면 0이 나온다. 따라서 정수이면 true, 소수이면 false를 반환한다.

```ts
Number(currentSendQuantity) === parseInt(currentSendQuantity, 10);
```
