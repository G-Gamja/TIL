# Uncaught TypeError: Cannot assign to read only property '0' of object '[object Array]'

오류가 발생하는 문장

```ts
currentCosmosActivities.sort((a, b) => (gt(a.timestamp, b.timestamp) ? -1 : 1));
```

---

이유

`currentCosmosActivities`는 읽기 전용 변수라 `sort`를 사용할 수 없다.

왜? -> `sort`는 원 배열을 변경하기 때문이다.

해결책

```ts
[...currentCosmosActivities].sort((a, b) =>
  gt(a.timestamp, b.timestamp) ? -1 : 1
);
```

---
