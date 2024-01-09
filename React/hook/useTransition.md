# useTransition

for concurrent rendering

디바운싱으로 지나친 렌더링을 막는 방식은 그 설정값의 설정이 애매함. 유저의 컴퓨터 성능에 따라 불필요한 디바운싱을 막을 수도 있고, 필요한 디바운싱을 막지 못할 수도 있음. 이를 해결하기 위해 useTransition이 나왔다.

https://velog.io/@ktthee/React-18-%EC%97%90-%EC%B6%94%EA%B0%80%EB%90%9C-useDeferredValue-%EB%A5%BC-%EC%8D%A8-%EB%B3%B4%EC%9E%90

```jsx
import "./App.css";
import { useState, useTransition } from "react"; // useTransition import 하기

let a = new Array(10000).fill(0);

function App() {
  let [name, setName] = useState(0);
  let [isPending, startTransition] = useTransition(); // 불러오기

  return (
    <div className="App">
      <input
        onChange={(e) => {
          startTransition(() => {
            // 성능 저하 일으키는 state 변경함수 감싸주기
            setName(e.target.value);
          });
        }}
      ></input>
      {a.map(() => {
        return <div>{name}</div>;
      })}
    </div>
  );
}

export default App;
```

https://velog.io/@brgndy/%EB%A6%AC%EC%95%A1%ED%8A%B8-useTransition-useDeferredValue-%EC%A0%95%EB%A6%AC

useDeferredValue vs useTransition
useDeferredValue와 useTransition hook은 상태변화의 우선순위를 낮게 하는 hook이다. 그렇다면 두 hook의 차이점은 무엇일까?

먼저 useDeferredValue는 값을 래핑해서 사용한다. 아래 예시 코드를 보면 count2라는 값을 래핑 해서 count2 값이 바뀌어도 deferredValue는 다른 상태변화가 전부 일어난 후에 바뀌게 된다. 즉, useDeferredValue는 값을 래핑 해서 값의 변화의 우선순위를 낮추도록 한다.

```ts
const deferredValue = useDeferredValue(count2);
```

다음은 useTransition hook이다. useTransition은 useDeferredValue와 다르게 값이 아닌, 함수를 래핑한다. 아래 예시 코드를 보면 () => { setText(e.target.value) } 함수를 래핑하고 있다. 래핑 된 함수의 우선순위를 낮춰서 다른 상태 변경이 전부 일어난 후 해당 함수를 실행하게 된다.

```ts
startTransition(() => {
  setText(e.target.value);
});
```

마치 useMemo는 값을 메모이제이션하고 useCallback은 함수를 메모이제이션 하는 것과 같이 값이냐 함수냐로 구분할 수 있다.

결론

- useDeferredValue는 상태 값에 우선순위를 낮추는 hook

- useTransition은 상태 변화를 일으키는 함수의 우선순위를 낮추는 hook이다
