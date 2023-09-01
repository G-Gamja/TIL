# Date 클래스

## unix타임 스탬프

```ts
const timeStamp = Number(
  // 1000을 곱해 밀리초 단위로 변환해줘야함
  times(BigInt(blockInfo.data.result.timestamp || "0").toString(10), "1000")
);

// 숫자타입이 들어가야함
const date = new Date(timeStamp);

return date.toLocaleString();
```
