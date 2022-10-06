# Hook [리액트 쿡북](https://ko.reactjs.org/docs/hooks-overview.html)

## How to type Hook
https://devtrium.com/posts/react-typescript-how-to-type-hooks

## useState훅 사용 예
https://velog.io/@velopert/using-hooks-with-typescript
# State Hook

함수형 컴포넌트에서 state변수를 관리할 수 있는 훅이다  

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
첫번째 인자: state 변수, 함수 호출 시 입력한 인수로 initialize됨  
두번째 인자: state 변수를 변경할 수 있는 함수.

## 여러개의 useState 훅 사용하기

리액트는 훅이 호출된 순서를 기억하기 때문에 여러개의 훅을 사용해도 괜찮다
```javascript
import React, { useState } from 'react';

function Example() {

  const [count, setCount] = useState(0);
  const [name, setName] = useState('');
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
      <button onClick={(e) => setName(e)}>
        Click me
      </button>
    </div>
  );
}
```  
## 훅 하나로 여러 state변수를 관리하기
두개의 state변수, 상태값을 하나의 객체로 관리할수도 있다

https://velog.io/@unknown9732/useState-%EC%97%AC%EB%9F%AC%EA%B0%9C%EB%A5%BC-%EC%93%B0%EB%8A%94-%EA%B2%BD%EC%9A%B0
```typescript
import React, { useState } from 'react';

function Example() {

  const [profile, setProfile] = useState({name:'',age:0});
  return (
    <div>
      <p>You clicked {count} times</p>
      <input type="text" value={state.name} onClick={e => setProfile({...profile, name: e.currenttarget.value})}>
        이름
      </input>
      <input type="number" value={state.age} onClick={e => setProfile({...profile, age: e.currenttarget.value})}>
        숫자
      </input>
    </div>
  );
}
```  
클래스형 컴포넌트의 setState메서드는 기존의 값과 입력된 값을 병합하지만
useState훅은 이전 상태값을 지우기 때문에 스프레드가 들어가야한다

https://ko.reactjs.org/docs/hooks-faq.html#should-i-use-one-or-many-state-variables
```  typescript 
const [editData, setEditData] = useState({ name: '', indexVal: 0 }); 
// "... state"를 spread 하여 name, indexVal가 "손실"되지 않음
setEditData({ ...editData, name: inputname, indexVal: inputindex });
```
useState에 타입 지정하기
======
https://velog.io/@jjburi/TypeScript-useState%EC%97%90%EC%84%9C-type-%EC%A7%80%EC%A0%95



## 방법 1 `useState<number>()`
`useState<number>()`
state변수의 타입을 정해주고 싶다면 위와 같이 Generic안에 타입을 정해준다
```typescript
type IUserInfo = {
  userid: string | undefined;
  username: string | undefined;
}
const [userInfo, setUserInfo] = useState<IUserInfo | null>(null);
```
## 방법 2 state변수가 구조를 가진 `객체` || `배열`
```typescript
type Todo = { id: number; text: string; done: boolean };
const [todos, setTodos] = useState<Todo[]>([]);
//Todo배열... / 그냥 배열이 아닌 Todo객체로 넘길거면 배열괄호 안써도 됨
```

상태변수 초기값 설정할 때 any[] 할당했다고 오류뜨면 as ~ 
```typescript
  const [userList, setUserList] = useState<userDataSetModel[]>(fetchedData ? (JSON.parse(fetchedData) as userDataSetModel[]) : []);
```
 사용된 as 는 Type Assertion 이라는 문법인데요, 특정 값이 특정 타입이다라는 정보를 덮어 쓸 수 있는 문법입니다. 

useState 응용 예제
=====
1.`useState` & 유저입력 저장(onChange) with input=text  
https://wonyoung2257.tistory.com/4  
실제로 구현에 참고한 것  https://react.vlpt.us/using-typescript/03-ts-manage-state.html

# `Effect Hook `| 함수형 컴포넌트에서 생명주기 함수 사용하기
`useEffect()` 함수는 React component가 렌더링 될 때마다 `특정 작업(Sied effect)`을 실행할 수 있도록 하는 리액트 Hook입니다.

API를 호출하고 해제하는 기능의 함수를 하나로 묶어서 관리할 수 있도록 해준다.
데이터 가져오기, 구독(subscription) 설정하기, 수동으로 React 컴포넌트의 DOM을 수정하는 것까지 이 모든 것이 side effects입니다. 이런 기능들(operations)을 side effect(혹은 effect)라 부르는 것

## 예시: 이펙트 훅에서 api호출하기
count는 api결과값을 저장할 상태값  
api결과 값을 setCount를 통해 count상태값에 저장->  
이때 훅에 입력된 함수는 렌더링시에만 호출되기 때문에 api통신이 불필요하게 일어나게 된다=> 훅의 두번째 매개변수로 값이 입력된 배열을 줌으로써 그 값(count)이 변경될때만 이팩트를 호출하도록 설정한다
```javascript
const [count, setCount] = useState(0);
useEffect(() => {
  setCount(1)
}, [count]);
```
## 예시: 훅을 통해 이벤트 처리함수를 등록하고 해제하기
1. 창 크기가 변경될때마다 onResize 함수가 호출되도록 등록한다
2. 이펙트 훅의 첫번째 매개변수에 등록된 함수가 또 다른 함수를 반환할 수 있다  
    반환된 함수는 컴포넌트가 unmount || 첫번째 매개변수 함수(이펙트 함수,addListener)가 호출되기 직전에 호출된다
3. 빈 배열을 넣음 == 값 업데이트 시점에는 호출X  
    1.컴포넌트가 마운트될 때에만 첫번째 매개변수 함수(이팩트 함수) 호출
    2.컴포넌트가 언마운트될 때에만 반환 함수(cleanup함수) 호출
```javascript
const [width, setWidth] = useState(window.innerWidth);
useEffect(() => {
  const onResize = ()=> setWidth(window.innerWidth);
  window.addEventListener('resize',onResize);//1
  return ()=>{//2
    window.removeEventListener('resize',onResize);
  };
}, []);//3
```
## useEffect()의 구조  
기본 형태:  
`useEffect(function,deps)`
- 함수: 수행하고자 하는 작업 | 렌더링 결과가 실제 돔에 반영된 후 호출된다  
    버튼을 클릭할 때마다 다시 렌더링되고-> 렌더링이 끝나면 훅에 입력된 함수가 실행된다
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
## useRef Hook: 특정 DOM에 접근해야 할 때
-----------
리액트를 사용하는 프로젝트에서도 가끔씩 DOM 을 직접 선택해야 하는 상황이 발생 할 때도 있습니다. 예를 들어서 특정 엘리먼트의 크기를 가져와야 한다던지, 스크롤바 위치를 가져오거나 설정해야된다던지, 또는 포커스를 설정해줘야된다던지 등 정말 다양한 상황이 있겠죠. 추가적으로 Video.js, JWPlayer 같은 HTML5 Video 관련 라이브러리, 또는 D3, chart.js 같은 그래프 관련 라이브러리 등의 외부 라이브러리를 사용해야 할 때에도 특정 DOM 에다 적용하기 때문에 DOM 을 선택해야 하는 상황이 발생 할 수 있습니다.  
이때 `ref`를 사용한다.

```javascript
//버튼 클릭시 인풋창에 포커스가 가도록 하는 예시
const nameInput = useRef();
//onReset실행시 ref가 가리키는 돔에 포커스
 const onReset = () => {
    // .current를 통해 해당 돔에 접근
    nameInput.current.focus();
  };
<input
        name="name"
        placeholder="이름"
        onChange={onChange}
        value={name}
        ref={nameInput}//이렇게 ref필드에 할당
      />
```
`useRef()` 를 사용하여 Ref `객체를` 만들고, 이 객체를 우리가 선택하고 싶은 `DOM` 에 `ref` 값으로 설정해주어야 합니다. 그러면, Ref 객체(여기서는 nameInput)의 .``current`` 값은 우리가 원하는 DOM 을 가르키게 됩니다.

ref는 아무리 컴포넌트가 렌더링되어도 값을 유지한다. 왜냐하면 이 값은 컴포넌트의 전 생애주기를 통해 유지되기 떄문
즉 컴포넌트가 마운트~언마운트까지 같은 값을 유지할 수 있음
### 부모가 자식의 ref를 받아내야 할 때
### `forwardedRef`
https://fettblog.eu/typescript-react-generic-forward-refs/  
https://ko.reactjs.org/docs/forwarding-refs.html   쿡북  


https://merrily-code.tistory.com/121


```javascript
return (
    <div className="App">
      <header className="App-header">
      // 컴포넌트에 직접 ref를 달아줌
        <CustomInput
          type="text"
          ref={nameRef} // 여기
          onKeyDown={onKeyDown}
          placeholder="이름을 입력하세요"
        ></CustomInput>
        <button ref={submitRef} onKeyDown={submitKeyDown}>
          제출
        </button>
      </header>
    </div>
  );
}
```
ref를 통해  `<CustomInput/>` 를 조작하려 하면 null값을 참조하려 했다는 오류가 발생!

### 원인 - 함수 컴포넌트는 ref가 존재하지 않음

ref값은 노드의 유형에 따라 다르다
* ref 속성이 html엘리먼트에 쓰였다면, 생성자에서 React.createRef()로 생성된 ref는 자신을 전달받은 dom엘리먼트를 current 프로퍼티 값으로 받는다.
* ref 속성이 커스텀 `클래스`컴포넌트에 쓰였다면, ref 객체는 마운트된 컴포넌트의 인스턴스를 current 프로퍼티 값으로서 받습니다.
* 함수 컴포넌트는 인스턴스가 없기 때문에 함수 컴포넌트에 ref 속성을 사용할 수 없음

then ref를 통해 함수 컴포넌트를 직접 제어하는건 불가능일까?

=> `React.forwardRef` 를 통해 해결
`React.forwardRef`를 사용하면 부모 컴포넌트로 -> 하위 컴포넌트로 ref전달 가능  
이렇게 전달받은 ref를 html 요소의 속성으로 넘겨줌으로써 함수 컴포넌트도 ref를 통해 돔의 제어가 가능해짐

```typescript
// props 목록 뒤에 ref를 별도로 전달받는 모습입니다.
const CustomInput = React.forwardRef(({ type, onKeyDown, placeholder }, ref) => {
  return (
    <input
      type={type}
      onKeyDown={onKeyDown}
      placeholder={placeholder}
      // 전달받은 ref는 HTML 속성으로 전달됩니다.
      ref={ref}
    ></input>
  );
});
export default CustomInput;

// 커스텀한 입력 컴포넌트
<CustomInput
  type="text"
  ref={nameRef} // 함수 컴포넌트는 ref가 존재하지 않음!
></CustomInput>

// 기본 형태
// forwardRef로 컴포넌트를 감싼 모습
const CustomInput = React.forwardRef({ type }, ref) => {
  return (
    <input
      type={type}
      // 전달받은 ref는 HTML 속성으로 전달됩니다.
      ref={ref}
    ></input>
  );
});

```
export default CustomInput;

## 리액트-훅-폼
원서: 
https://react-hook-form.com/kr/get-started/  

한글 블로그: https://velog.io/@ckm960411/react-hook-form-TypeScript-%EB%A6%AC%EC%95%A1%ED%8A%B8-%ED%9B%85-%ED%8F%BC-%EC%82%AC%EC%9A%A9%EB%B2%95%EA%B3%BC-%ED%83%80%EC%9E%85-%EC%A3%BC%EA%B8%B0


도입 이와 이슈들, 도입 과정이 상세하게 정리되어있음: https://tech.inflab.com/202207-rallit-form-refactoring/react-hook-form/

## 사용 규칙
Hook 사용 규칙
Hook은 그냥 JavaScript 함수이지만, 두 가지 규칙을 준수해야 합니다.

최상위(at the top level)에서만 Hook을 호출해야 합니다. `반복문, 조건문, 중첩된 함수` 내에서 Hook을 실행하지 마세요.  
   `함수 컴포넌트  & 커스텀 훅` 안에서만 Hook을 호출해야 합니다.  
    ---일반 JavaScript 함수--- 에서는 Hook을 호출해서는 안 됩니다.


