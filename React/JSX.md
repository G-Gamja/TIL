# JSX란 무엇인가

https://goddaehee.tistory.com/296

```
<div className="box>
    <Title text="helloworld", width={200}>
</div>
```
Title은 리액트 컴포넌트로 JSX에서는 돔 요소와 리액트 컴포넌트를 같이 사용할 수 있다.
width처럼 문자열 리터럴이 아닌 속성값은 중괄호를 사용해 입력한다. 이 컴포넌트가 어떤 돔 요소로 치환될지는 Title컴포넌트의 렌더 함수가 결정한다.