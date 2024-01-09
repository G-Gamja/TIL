# JavaScript에서 Context 이해하기

JavaScript에서 `context`는 코드에서 현재 실행 중인 코드의 문맥 또는 환경을 나타냅니다. 이는 함수가 호출될 때, 그 함수가 어디에서 호출되었는지에 따라서 달라집니다.

## 1. 함수의 Context

JavaScript에서 함수는 일반적으로 세 가지 방법으로 호출됩니다.

### a. 일반 함수 호출

```javascript
function exampleFunction() {
  console.log(this); // 'this'는 전역 객체를 가리킴
}

exampleFunction();
```

### b. 메서드 호출

```javascript
const obj = {
  exampleMethod() {
    console.log(this); // 'this'는 호출한 객체를 가리킴
  },
};

obj.exampleMethod();
```

### c. 생성자 함수 호출

```javascript
function ExampleConstructor() {
  console.log(this); // 'this'는 생성된 인스턴스를 가리킴
}

const instance = new ExampleConstructor();
```

## 2. JavaScript에서의 `this`

JavaScript에서 `this`는 실행 컨텍스트에 따라 동적으로 결정됩니다. 이것이 바로 함수가 어떻게 호출되었느냐에 따라 `this`가 가리키는 대상이 달라짐을 의미합니다.

## 3. React에서의 Context

React에서는 `React.createContext`를 사용하여 컴포넌트 트리 전체에서 전역적으로 사용할 수 있는 값을 공유할 수 있습니다.

```javascript
import React, { createContext, useContext } from "react";

// 컨텍스트 생성
const MyContext = createContext();

// 프로바이더 컴포넌트 생성
function MyProvider({ children }) {
  const sharedValue = "This is a shared value";

  return (
    <MyContext.Provider value={sharedValue}>{children}</MyContext.Provider>
  );
}

// 자식 컴포넌트에서 컨텍스트 사용
function MyComponent() {
  const contextValue = useContext(MyContext);

  return <p>{contextValue}</p>;
}
```

위 예제에서 `MyComponent`는 `MyContext`에서 제공되는 값을 소비합니다. `MyProvider`로 감싸진 컴포넌트 트리 내에서는 모든 하위 컴포넌트에서 이 값을 공유할 수 있습니다.

React의 Context는 상태 관리, 테마 전달, 사용자 인증 등에서 유용하게 활용됩니다.

### 예시

```javascript
import { createContext, useContext } from "react";
const MtContext = createContext();

function App() {
  return (
    <MtContext.Provider value="Hello World">
      <Parent />
    </MtContext.Provider>
  );
}

function Parent() {
  return <Child />;
}

function Child() {
  const value = useContext(MtContext);
  return <div>{value}</div>;
}
```
