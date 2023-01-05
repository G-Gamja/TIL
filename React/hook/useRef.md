# useRef

- useRef를 통해 생성한 변수는 상태값이 변해도 즉시 컴포넌트를 리랜더링시키지 않는다

ref변수를 여러개 생성하기
```jsx
[리팩토링 전]
const firstTab = useRef();
const secondTab = useRef();
const thirdTab = useRef();

.
.
.

<TabContents ref={firstTab}>...</TabContents>
<TabContents ref={secondTab}>...</TabContents>
<TabContents ref={thirdTab}>...</TabContents>

[리팩토링 후]
const tabRef = useRef([]);

.
.
.

<TabContents ref={el => (tabRef.current[0] = el)}>...</TabContents>
<TabContents ref={el => (tabRef.current[1] = el)}>...</TabContents>
<TabContents ref={el => (tabRef.current[2] = el)}>...</TabContents>
```

참고 (https://velog.io/@dosilv/ReactWeb-API-useRef-scrollIntoView)