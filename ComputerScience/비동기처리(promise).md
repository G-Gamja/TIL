[Javascript] 비동기, Promise, async, await 확실하게 이해하기

https://elvanov.com/2597

new Promise가 선언 된 직 후 비동기 작업이 실행되며 비동기 작업을 성공으로 간주하고 싶을 때 resolve를 호출하고, 실패라 간주하고 싶다면 reject 함수를 호출합니다. 이 비동기 작업이 성공했을 때 후속 조치를 지정하고 싶다면 then으로, 실패 시의 후속 조치는 catch 로 지정한다

## Promise

프로미스 생성자 함수에 인자로 받는 함수를 `executor` 라는 이름으로 부릅니다. 이 함수에 대해 더 자세히 설명하면 다음과 같습니다.

`executor는` 첫번째 인수로 `resolve` 이며, 두번째 인수로 `reject` 를 받습니다. (이름은 우리가 정의하는 거라서 아무렇게나 해도 사실 상관은 없지만, 국룰을 따르도록 합시다.)  
`resolve` 는 `executor` 내에서 호출할 수 있는 또 다른 함수입니다.  
 resolve 를 호출하게 된다면 “이 비동기 작업이 성공했어!” 라는 뜻입니다.
`reject` 또한 executor 내에서 호출할 수 있는 또 다른 함수입니다.  
`reject` 를 호출하게 된다면 “이 비동기 작업이 실패했어…” 라는 뜻입니다

- `resolve`: executor안에서 호출되면 비동기 작업이 성공했다는 뜻/ .then 이 실행
- `reject`:xecutor안에서 호출되면 비동기 작업이 실패했다는 뜻/ .catch 가 실행
- `then` 메소드는 해당 Promise 가 성공했을 때의 동작을 지정합니다. 인자로 함수를 받습니다.
- `catch` 메소드는 해당 Promise 가 실패했을 때의 동작을 지정합니다. 인자로 함수를 받습니다.

```javascript
const promise1 = new Promise((resolve, reject) => {
  resolve();
});
promise1
  .then(() => {
    // 얘가 실행됨
    console.log("then!");
  })
  .catch(() => {
    console.log("catch!");
  });
```

### 특징들

- executor 내부에서 에러가 throw 된다면 해당 에러로 reject 가 수행됩니다.
- executor 의 리턴 값은 무시됩니다.
- 첫 번째 reject 혹은 resolve 만 유효합니다. (두 번째부터는 무시됩니다. 이미 해당 함수가 호출되었다면 throw 또한 무시됩니다.)

```javascript
// catch 로 연결됩니다.
const throwError = new Promise((resolve, reject) => {
  throw Error("error");
});
throwError
  .then(() => console.log("throwError success"))
  .catch(() => console.log("throwError catched"));

// 아무런 영향이 없습니다.
const ret = new Promise((resolve, reject) => {
  return "returned";
});
ret
  .then(() => console.log("ret success"))
  .catch(() => console.log("ret catched"));

// resolve 만 됩니다.
const several1 = new Promise((resolve, reject) => {
  resolve();
  reject();
});
several1
  .then(() => console.log("several1 success"))
  .catch(() => console.log("several1 catched"));

// reject 만 됩니다.
const several2 = new Promise((resolve, reject) => {
  reject();
  resolve();
});
several2
  .then(() => console.log("several2 success"))
  .catch(() => console.log("several2 catched"));

// resolve 만 됩니다.
const several3 = new Promise((resolve, reject) => {
  resolve();
  throw new Error("error");
});
several3
  .then(() => console.log("several3 success"))
  .catch(() => console.log("several3 catched"));
```

## async 함수

기본 형식은 다음과 같다

- try안에는 오류가 발생할 수도 있는 구문을 집어넣고 catch에서는 발생한 오류를 다룬다
- await는 비동기 작엄의 완료까지 해당 비동기 작업 단위를 잠시 멈추는 것으로 비동기 작업 환경 안에서 동기 작업환경과 유사하게 구현할 수 있도록 해준다.
- 예시로 어떤 데이터를 가져온 후에 후속 작업이 가능한 경우에 await로 동기처럼 조작할 수 있겠다.
- await가 던진 에러는 아래 catch구문에서 잡아준다
- then은 그닥 필요가 없음, 어떤게 성공한 뒤 어떤 작업을 해준다... 그냥 await로 걸어놓고 다음 작업 하면 알아서 전 작업이 끝난 후에 다음 작업으로 넘어가니까...

```javascript
async function f() {
  try {
    let response = await fetch("http://유효하지-않은-주소");
    let user = await response.json();
  } catch (err) {
    alert(err); // TypeError: failed to fetch
  }
}

f();
```

```javascript
// 기존
// function startAsync(age) {
//  return new Promise((resolve, reject) => {
//    if (age > 20) resolve(`${age} success`);
//   else reject(new Error("Something went wrong"));
//  });
// }

async function startAsync(age) {
  if (age > 20) return `${age} success`;
  else throw new Error(`${age} is not over 20`);
}

setTimeout(() => {
  const promise1 = startAsync(25);
  promise1
    .then((value) => {
      console.log(value);
    })
    .catch((error) => {
      console.error(error);
    });
  const promise2 = startAsync(15);
  promise2
    .then((value) => {
      console.log(value);
    })
    .catch((error) => {
      console.error(error);
    });
}, 1000);
```

### async 함수의 리턴값은 무조건 Promise이다

그렇기에 async 함수를 실행시킨 뒤에는 then, catch를 사용해주어야한다.

## await: promise작업단위가 끝날 때 까지 기달려라

- aawait 은 Promise 가 resolve 한 값을 내놓습니다. async 함수 내부에서는 리턴하는 값을 resolve 한 값으로 간주하므로, `${age} success` 가 value로 들어온다는 점을 알 수 있습니다.
- 해당 Promise 에서 reject 가 발생한다면 예외가 발생합니다. 이 예외 처리를 하기 위해 try-catch 구문을 사용했습니다. reject 로 넘긴 에러(async 함수 내에서는 throw 한 에러)는 catch 절로 넘어갑니다. 이로써 본래 해왔던 에러 처리 하듯이 진행할 수 있습니다.

```javascript
async function startAsync(age) {
  if (age > 20) return `${age} success`;
  else throw new Error(`${age} is not over 20`);
}

async function startAsyncJobs() {
  const promise1 = startAsync(25);
  try {
    // resolve 가 리턴됨: 성공한다면
    // 만약 실패한다면 캐치 구문으로 들어감
    const value = await promise1;
    console.log(value);
  } catch (e) {
    console.error(e);
  }
  const promise2 = startAsync(15);
  try {
    const value = await promise2;
    console.log(value);
  } catch (e) {
    console.error(e);
  }
}
```

await는 비동기 환경에서 동기환경처럼 코딩하기 위해서 사용된다

#### 요론 방식도 있더라...

```javascript
(async () => {
  let response = await fetch('/article/promise-chaining/user.json');
  let user = await response.json();
  ...
})();

```

```
async function f() {
  await Promise.reject(new Error("에러 발생!"));
}
```

는 아래에 동일함

```
async function f() {
  throw new Error("에러 발생!");
}
```

## 여러 Promise작업을 기다려야 한다면

여기서 하나라도 에러가 발생한다면 바로 에러가 발생되고 마찬가지로 catch구문을 통해 에러를 핸들링할 수 있다.

```javascript
// 프라미스 처리 결과가 담긴 배열을 기다립니다.
let results = await Promise.all([
  fetch(url1),
  fetch(url2),
  ...
]);
```

## Promis.then.catch VS async, await

아래의 두 코드들은 동일하다

```javascript
function loadJson(url) {
  return fetch(url).then((response) => {
    if (response.status == 200) {
      return response.json();
    } else {
      throw new Error(response.status);
    }
  });
}

loadJson("no-such-user.json").catch(alert); // Error: 404
```

```javascript
async function loadJson(url) {
  // (1)
  let response = await fetch(url); // (2)

  if (response.status == 200) {
    let json = await response.json(); // (3)
    return json;
  }

  throw new Error(response.status);
}

loadJson("no-such-user.json").catch(alert); // Error: 404 (4)
```

# Promise.all()

Promise.all은 요청 중에 하나라도 요청 거부가 되면 성공한 객체들은 모두 무시하고 error객체 하나만 넘긴다.

이 메서드는 여러 프로미스의 결과를 집계할 때 유용하게 사용할 수 있습니다. 일반적으로 다음 코드를 계속 실행하기 전에 서로 연관된 비동기 작업 여러 개가 모두 이행되어야 하는 경우에 사용됩니다.

입력 값으로 들어온 프로미스 중 하나라도 거부 당하면 Promise.all()은 즉시 거부합니다. 이에 비해, Promise.allSettled()가 반환하는 프로미스는 이행/거부 여부에 관계없이 주어진 프로미스가 모두 완료될 때까지 기다립니다. 결과적으로, 주어진 이터러블의 모든 프로미스와 함수의 결과 값을 최종적으로 반환합니다.

이행
반환한 프로미스의 이행 결과값은 (프로미스가 아닌 값을 포함하여) 매개변수로 주어진 순회 가능한 객체에 포함된 모든 값을 담은 배열입니다.

빈 객체를 전달한 경우, (동기적으로) 이미 이행한 프로미스를 반환합니다.
전달받은 모든 프로미스가 이미 이행되어 있거나 프로미스가 없는 경우, 비동기적으로 이행하는 프로미스를 반환합니다.
거부
주어진 프로미스 중 하나라도 거부하면, 다른 프로미스의 이행 여부에 상관없이 첫 번째 거부 이유를 사용해 거부합니다.

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/all

# Promise.allSettled

사용 예

```ts
export type ErrorResponse = {
  code: number;
  message: string;
};

export type Result<T> = {
  data: {
    jsonrpc: string;
    id: string;
    result?: T;
    error?: ErrorResponse;
  };
  response_time: number;
};

type UseGetLatestCheckPointSWR = {
  rpcURLList: string[];
};

type FetchParam = {
  requestURLs: string[];
};

export function useGetLatestCheckPointSWR(
  { rpcURLList }: UseGetLatestCheckPointSWR,
  config?: SWRConfiguration
) {
  const chain = useRecoilValue(getChainInstanceState);

  const makeBodyData = (fetchUrl: string) => ({
    jsonData: {
      id: fetchUrl,
      jsonrpc: "2.0",
      method: "sui_getLatestCheckpointSequenceNumber",
      params: [],
    },
  });

  const fetcher = (fetchUrl: string): Promise<Result<string>> =>
    chain.urlRequester.fetcher(
      `https://api.mintscan.io/call?link=${fetchUrl}`,
      "POST",
      makeBodyData(fetchUrl)
    );

  const multiFetcher = ({ requestURLs }: FetchParam) =>
    Promise.allSettled(requestURLs.map((url) => fetcher(url)));

  const { data, error, mutate } = useSWR<
    PromiseSettledResult<Result<string>>[]
  >({ requestURLs: rpcURLList }, multiFetcher, {
    refreshInterval: 0,
    revalidateOnFocus: false,
    isPaused: () => !rpcURLList,
    ...config,
  });

  return { data, error, mutate };
}

const { data: checkpointDatas } = useGetLatestCheckPointSWR(
  { rpcURLList: rpcAddressList },
  config
);

const rpcEndPoints = useMemo(
  () =>
    relocatedRpcDatas.map((item) => {
      const checkpointData = checkpointDatas?.find(
        (a) => a.status === "fulfilled" && a.value.data.id === item.rpc
      );
      return {
        ...item,
        checkPoint:
          checkpointData?.status === "fulfilled"
            ? Number(checkpointData.value.data.result)
            : 0,
        responseTime:
          checkpointData?.status === "fulfilled"
            ? checkpointData.value.response_time
            : 0,
        status: checkpointData?.status === "fulfilled",
      };
    }),
  [relocatedRpcDatas, checkpointDatas]
);
```

# Redundant use of `await` on a return value.

https://stackoverflow.com/questions/44806135/why-no-return-await-vs-const-x-await

# Promise 와 async,await차이

https://velog.io/@pilyeooong/Promise%EC%99%80-asyncawait-%EC%B0%A8%EC%9D%B4%EC%A0%90
