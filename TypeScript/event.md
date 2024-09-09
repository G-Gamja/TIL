https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/

# 이벤트 리스너

이벤트 리스너에 add했을때 remove할 때 이렇게 객체를 제거해야 제거가 가능

```js
const a = (event) => console.log(event);
addEventListener("message", a);

dispatchEvent(new Event("message"));

removeEventListener("message", a);
```

이거는 제거가 안됨.

```js
addEventListener("message", (event) => console.log(event));

dispatchEvent(new Event("message"));

removeEventListener("message", (event) => console.log(event));
```

```js
const test1 = () => {
  console.log("test");
};

const test2 = () => {
  console.log("test");
};

// false 출력
console.log(test1 === test2);
```
