# State Hook [리액트 쿡북](https://ko.reactjs.org/docs/hooks-overview.html)

```javascript
import React, { useState } from 'react';

function Example() {
  // "count"라는 새 상태 변수를 선언합니다
  const [count, setCount] = useState(0);
    //인자로 넘기는 0은 상태 변수의 초기값을 선언한 것이다.
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

응용 예제
=====
1.`useState` & 유저입력 저장(onChange) with input=text  
https://wonyoung2257.tistory.com/4  
실제로 구현에 참고한 것  https://react.vlpt.us/using-typescript/03-ts-manage-state.html

# `Effect Hook `
`useEffect()` 함수는 React component가 렌더링 될 때마다 `특정 작업(Sied effect)`을 실행할 수 있도록 하는 리액트 Hook입니다.

데이터 가져오기, 구독(subscription) 설정하기, 수동으로 React 컴포넌트의 DOM을 수정하는 것까지 이 모든 것이 side effects입니다. 이런 기능들(operations)을 side effect(혹은 effect)라 부르는 것이 익숙하지 않을 수도 있지만, 아마도 이전에 만들었던 컴포넌트에서 위의 기능들을 구현해보았을 것입니다.

## useEffect()의 구조  
기본 형태:  
`useEffect(function,deps)`
- 함수: 수행하고자 하는 작업
- deps(dependency): 배열 형태로 할당되며, 배열안에는 검사하고자 하는 특정 값 혹은 빈 배열을 할당하게 된다. 물론 할당하지 않아도 된다  

### 컴포넌트가 마운트 되었을 때 (첫 렌더링 시점)    
컴포넌트가 화면에 `첫 등장`할 때만 이펙트를 실행시키고 싶다면 2번째 인자에 `빈 배열`을 넣는다
```javascript
useEffect(() => {
  console.log('마운트 될 때만 실행되는 코드')
}, []);//빈 배열이 들어감 
```
## 리렌더링 될떄마다 이펙트 함수 호출
`배열을 생략`하게 된다면 `매 랜더링`마다 이펙트가 실행된다
```javascript
useEffect(() => {
  console.log('렌더링 될 때마다 실행')
},);//배열 자체가 할당되지 않음
```
### 1.컴포넌트가 update 되었을 때 (특정 값(state변수등)이 바뀔때)  
`특정값`(count변수)가 변경되었을 때마다 이펙트를 실행시키고 싶다면 배열에 해당 값을 넣어준다  
특정값의 업데이트뿐만 아니라 마운트시에도 실행된다
```javascript
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]);//count변수가 update될 때마다 이펙트가 실행됨 
```
특정값의 업데이트뿐만 아니라 마운트시에도 실행된다. 오직 업데이트시에만 이펙트를 실행시키고 싶다면 다음의 코드를 사용한다
 
컴포넌트가 마운트되는 시점을 조건문을 통해 구현
```javascript
const mounted = useRef(false);

useEffect(() => {
  if(!mounted.current){
    mounted.current = true;
  }else{
    //이 곳에서 이펙트 함수 구현
  }
}, [deps값]);
```  

## 컴포넌트가 `unmount`되는 시점 & `update`직전
* cleanup 함수: return뒤에 나오는 함수이며 컴포넌트가 사라지는 시점에 실행되는 함수
* `언마운트`시에만 cleanup함수를 실행하고 싶을때  
  * 두번째 파라미터에 `빈 배열[]`을 넣는다
* deps값 `update`시에만 cleanup함수를 실행하고 싶을때  
  * 두번째 파라미터에 `특정값이 들어있는 배열 [deps]`을 넣는다
## deps에 특정값을 넣는다는 것은...  
컴포넌트가 처음 마운트되는 시점 + 지정한 값의 update + 언마운트될 때 + 값이 바뀌기 직전  
에 호출된다
## 과도한 렌더링 방지
이펙트 훅은 매 렌더링마다 새 이펙트로 바꿔 실행되는데 이때 불필요하게 렌더링
되는 경우 부하가 올 수 있다 이럴 때 쓸 수 있는 방법
```javascript
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // count가 바뀔 때만 effect를 재실행합니다. []이면 첫 마운트 시때만 렌더링 되며 아무 인자도 넘기지 않는다면 매 렌러딩마다 훅이 발동한다
```
##  사용 예 2
## clean up 함수
Component가 Unmount 되었을 때(사라질 때) & Update 되기 직전에 사용되어야 할 함수를 Clean up 함수라 일컬으며 return에 들어가는 함수를 말한다

useEffect는 함수를 반환할 수 있는데 이 함수를 cleanup이라고 합니다.

Unmount 될 때만 cleanup 함수를 실행시키고 싶다면 deps에 빈 배열을,
특정 값이 업데이트되기 직전에 cleanup 함수를 실행시키고 싶다면 deps에 해당 값을 넣어주면 됩니다.
```javascript
useEffect(() => {
  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
  //언마운트될때 리턴 함수가 실행된다:clean up함수
  return () => {
    ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
  };
}, [props.friend.id]); // props.friend.id가 바뀔 때만 재구독합니다.
```
1. Component가 Mount 되었을 때(나타날 때)
```javascript
useEffect(() => {
    console.log("렌더링 될때마다 실행");
  });
deps부분을 생략한다면 해당 컴포넌트가 렌더링 될 때마다 useEffect가 실행되게 됩니다. 만약 맨 처음 렌더링 될 때 한 번만 실행하고 싶다면 deps위치에 빈 배열을 넣어줍니다.

  useEffect(() => {
    console.log("맨 처음 렌더링될 때 한 번만 실행");
  },[]);
```

## 사용 규칙
Hook 사용 규칙
Hook은 그냥 JavaScript 함수이지만, 두 가지 규칙을 준수해야 합니다.

최상위(at the top level)에서만 Hook을 호출해야 합니다. 반복문, 조건문, 중첩된 함수 내에서 Hook을 실행하지 마세요.
React 함수 컴포넌트 내에서만 Hook을 호출해야 합니다. 일반 JavaScript 함수에서는 Hook을 호출해서는 안 됩니다. (Hook을 호출할 수 있는 곳이 딱 한 군데 더 있습니다. 바로 직접 작성한 custom Hook 내입니다. 이것에 대해서는 나중에 알아보겠습니다.)
이 규칙들을 강제하기 위해서 linter plugin을 제공하고 있습니다. 이 규칙들이 제약이 심하고 혼란스럽다고 처음에는 느낄 수 있습니다. 하지만 이것은 Hook이 제대로 동작하기 위해서는 필수적인 조건입니다.

