# useSWR 훅이란

데이터를 가져오는 axios의 get같은 메서드인데 설정된 주기값으로 값을 알아서
다시 가져오기 때문에 편리하다
또 swr은 바뀌었다는걸 알아서 보고 값을 알아서 가져와 줌(캐싱기능)

# 왜 사용하는가?

참조: https://doqtqu.tistory.com/329

참조 (서스펜스 적용):https://tecoble.techcourse.co.kr/post/2021-07-11-suspense/

# parameter중 key에 대해

```ts
const { data, isValidating, error } = useSWR<ReturnDataType>(
    'key', async () => {
      // 비동기 통신 부분
      const res = await ...
      return res.data
    },
  );
```

여기서 'key'가 중요한건데 나는 요 키 가 그냥 단순히 fetcher의 아규먼트로 취급하는 줄
알아서 파라미터가 없는 fetcher에 아무 키도 안넣었더니 오류를 뱉어내더라고? 그래서 다른 훅(파람이 필요없는 fetcher를 쓰는 SWR훅)이랑 같은 값('{}')을 넣었더니 아니 그 훅의 값을 가져오는 것이다.

그래서 대충 아무 스트링값이나 고유한 값을 줬더니 잘 가져오더라고...이게 뭐람

# Option -

원래 처음 데이터를 가져올때 데이터를 받아오는 중이면 undefine처리가 돼서 데이터를 받는 쪽에서 동기적으로 처리하기
어려울 떄가 있는데 useSWR로 데이터 가져올때 동기적으로 가져오려면 (서스펜스 옵션을 켜서 그냥 undefine 처리 안해버리도록)

항상 데이터를 가져오게 되며 undefine처리를 할 필요가 없지만, 에러 바운더리는 설정을 해줘야 데이터 못가져왔을때 에러처리가 된다  
가져온 데이터가 항상 렌더링 될 준비 (undefine의 가능성이 없다는 소리)가 되어있다.
In Suspense mode, data is always the fetch response (so you don't need to check if it's undefined). But if an error occurred, you need to use an error boundary to catch it:

Normally, when you enabled suspense it's guaranteed that data will always be ready on render:

- Suspense mode suspends rendering until the data is ready

_ ### 옵션 목록
_ 참고: https://swr.vercel.app/ko/docs/api

_ suspense = false: React Suspense 모드를 활성화 (상세내용)
_ fetcher(args): fetcher 함수  
_ revalidateIfStale = true: stale 데이터가 존재할 경우 마운트시에 자동으로 갱신 (상세내용)
_ revalidateOnMount: 컴포넌트가 마운트되었을 때 자동 갱신 활성화 또는 비활성화
_ revalidateOnFocus = true: 창이 포커싱되었을 때 자동 갱신 (상세내용)
_ revalidateOnReconnect = true: 브라우저가 네트워크 연결을 다시 얻었을 때 자동으로 갱신(navigator.onLine을 통해) (상세내용)
_ refreshInterval (상세내용):
_ 기본적으로는 비활성화: refreshInterval = 0
_ 숫자 설정 시, 인터벌 폴링
_ 함수 설정 시, 함수는 최신 데이터를 수신하고 간격을 밀리초 단위로 반환
_ refreshWhenHidden = false: 창이 보이지 않을 때 폴링(refreshInterval이 활성화된 경우)
_ refreshWhenOffline = false: 브라우저가 오프라인일 때 폴링(navigator.onLine에 의해 결정됨)
_ shouldRetryOnError = true: fetcher에 에러가 있을 때 재시도
_ dedupingInterval = 2000: 이 시간 범위내에 동일 키를 사용하는 요청 중복 제거
_ focusThrottleInterval = 5000: 이 시간 범위 동안 단 한 번만 갱신
_ loadingTimeout = 3000: onLoadingSlow 이벤트를 트리거 하기 위한 타임아웃
_ errorRetryInterval = 5000: 에러 재시도 인터벌
_ errorRetryCount: 최대 에러 재시도 수
\_ fallback: 다중 폴백 데이터의 키-값 객체 (예시)

`fallback: ()=> null,`
이렇게 하면 fetcher에서 잘못가져왔을 때 아예 터져버리게 만들어버림

_ fallbackData: 반환될 초기 데이터(노트: hook 별로 존재)
_ onLoadingSlow(key, config): 요청을 로드하는 데 너무 오래 걸리는 경우의 콜백 함수(loadingTimeout을 보세요)
_ onSuccess(data, key, config): 요청이 성공적으로 종료되었을 경우의 콜백 함수
_ onError(err, key, config): 요청이 에러를 반환했을 경우의 콜백 함수
_ onErrorRetry(err, key, config, revalidate, revalidateOps): 에러 재시도 핸들러
_ compare(a, b): 비논리적인 리렌더러를 회피하기 위해 반환된 데이터가 변경되었는지를 감지하는데 사용하는 비교 함수. 기본적으로 dequal을 사용
_ isPaused(): 갱신의 중지 여부를 감지하는 함수. true가 반환될 경우 가져온 데이터와 에러는 무시합니다. 기본적으로는 false를 반환
_ use: 미들웨어 함수의 배열 (상세내용)
_ 느린 네트워크(2G, <= 70Kbps)에서는, 기본적으로 errorRetryInterval이 10초이며, loadingTimeout은 5초
_ 옵션 필드 목록: https://doqtqu.tistory.com/329#4.3.%20Options

참조: https://swr.vercel.app/docs/suspense

한글 api설명서: https://swr.vercel.app/ko/docs/global-configuration

# 뮤테이션

https://velog.io/@code-bebop/SWR-%EC%8B%AC%EC%B8%B5%ED%83%90%EA%B5%AC

# dynamic endpoints

multi fetcher
https://github.com/vercel/swr/discussions/475

# 주의할점

여러 훅을 동시에 쓸 때
한 훅의 리턴값으로 다른 훅의 파라미터를 사용할때
이때 실행될 훅의 파라미터가 pause조건으로 걸려있을 때
첫 훅의 리턴값이 없을때는 항상 pause가 걸리기에 모든 동작이 스탑될거다...

- 그니까 pause조건과 파람의 옵셔널을 잘 생각해야한다.

# Error시 key 변경해서 재시도 하는방법

```tsx
const fetcher = async (fetchUrl: string): Promise<NFTMetaPayload | null> => {
  try {
    if (!httpsRegex.test(fetchUrl)) {
      return null;
    }

    if (nftSourceURI.error) {
      throw nftSourceURI.error;
    }
    return await get<NFTMetaPayload>(fetchUrl);
  } catch (e) {
    if (isAxiosError(e)) {
      if (e.response?.status === 404) {
        // NOTE 여기서 key를 변경해서 재시도를 해야함
        return fetcher(
          nftSourceURI.data?.token_uri.includes("ipfs:")
            ? convertIpfs(nftSourceURI.data?.token_uri)
            : convertIpfs(nftSourceURI.data?.token_uri)
        );
      }
    }
    throw e;
  }
};

const { data, isValidating, error, mutate } = useSWR<
  NFTMetaPayload | null,
  AxiosError
>(paramURL, fetcher, {
  revalidateOnFocus: false,
  revalidateIfStale: false,
  revalidateOnReconnect: false,
  errorRetryCount: 3,
  errorRetryInterval: 5000,
  onErrorRetry: (err, _, __, revalidate, { retryCount }) => {
    // 404에서 재시도 안함
    if (err.response?.status === 404) return;

    // 10번까지만 재시도함
    if (retryCount >= 10) return;

    // 5초 후에 재시도
    setTimeout(() => {
      void revalidate();
      w;
    }, 2000);
  },
  isPaused: () => !paramURL || !nftSourceURI.data,
  ...config,
});
```
