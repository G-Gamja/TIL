# Axios error 메시지 얻기

swr로 호출한 axios fetcher의 에러 객체는 다음과 같이 접근 가능하다

oneInchRoute.error.response

response의 field내에 data라는 필드 안에서 이제 호출한 fetcher의 에러 메시지나 등등을 가져올 수 있는데 이때 아래와 같이 에러의 제네릭 타입을 설정해줘야 접근이 가능

```ts
type OneInchSwapError = {
  description: string;
  error: string;
  meta: [
    {
      type: string;
      value: string;
    },
  ];
  requestId: string;
  statusCode: number;
};

  const { data, isValidating, error, mutate } = useSWR<OneInchSwapPayload, AxiosError<OneInchSwapError>>(requestURL, fetcher, {
    revalidateOnFocus: false,
    dedupingInterval: 14000,
    refreshInterval: 15000,
    errorRetryCount: 0,
    isPaused: () => !swapParam,
    ...config,
  });

if (oneInchRoute.error) {
    // OneInchSwapError의 타입을 가져오는 것
    return oneInchRoute.error.response?.data.description;
}
```