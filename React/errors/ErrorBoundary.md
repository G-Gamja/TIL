# 네트워크를 다 끊어도 ErrorBoundary(에러 바운더리)가 작동을 하지 않아요~!

내가 해결한 방법은 아래와 같아요.

1. 컴포넌트 내 swr훅에 suspense적용(꼭 해줘야함)
2. 그 훅에 throw error를 해줘야 에러바운더리에서 캐리를 한다 이말이야
   - 나는 기존에 try/catch에서 에러가 나면 null을 리턴하도록 해서 에러바운더리가 캐치를 못한것임

- ps 에러바운더리에서 reset시킬때도 그 훅에 throw error를 해줘야한다. 안하면 에러바운더리가 reset을 안한다.
  비교해보자

### 기존

```typescript
const fetcher = async (fetchUrl: string) => {
  try {
    return await get<NFTInfoPayload>(fetchUrl);
  } catch (e: unknown) {
    return null;
  }
};
```

### 바뀐 후

```typescript
const fetcher = async (fetchUrl: string) => {
  try {
    return await get<NFTInfoPayload>(fetchUrl);
  } catch (e) {
    if (isAxiosError(e)) {
      if (e.response?.status === 404) {
        return null;
      }
    }
    throw e;
  }
};
```
