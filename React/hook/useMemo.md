# useMemo
`useMemo` 의 첫번째 파라미터에는 어떻게 연산할지 정의하는 함수를 넣어주면 되고 두번째 파라미터에는 deps 배열을 넣어주면 되는데, 이 배열 안에 넣은 내용이 바뀌면, 우리가 등록한 함수를 호출해서 값을 연산해주고, 만약에 내용이 바뀌지 않았다면 이전에 연산한 값을 재사용하게 됩니다.

# 왜 쓸까?

기본적으로 react는 부모 component로부터 받는 state,props가 변동될 경우 rerendering됩니다. 당연한 것이죠. 하지만 여기에는 분명한 문제가 존재합니다.

예를들어 state,props가 여러가지라면?
state1, state2, state3이 존재하는 상태에서 state1을 변경시켜주었는데 state2, state3도 재계산된다면??
이것이 과연 좋은 rerendering일까요??
```javascript
function MyComponent({ x, y }) {
  const z = useMemo(() => compute(x, y), [x, y]);
  return <div>{z}</div>;
}
```
x와 y 값이 이 전에 랜더링했을 때와 동일할 경우, 이 전 랜더링 때 저장해두었던 결과값을 재활용합니다.   
하지만, x와 y 값이 이 전에 랜더링했을 때와 달라졌을 경우, () => compute(x, y) 함수를 호출하여 결과값을 새롭게 구해 z에 할당해줍니다.
* x,y를 주시하다가 값이 변경되면 -> compute함수를 실행
*                변경되지 않으면 -> 이전 계산된 값을 실행 


참조: https://velog.io/@kysung95/%EC%A7%A4%EB%A7%89%EA%B8%80-useMemo  
참조: https://www.daleseo.com/react-hooks-use-memo/
# 주의할 것 async는 안돼요!

  // STUB - 비동기 함수 사용시 useMemo와는 함꼐 사용하지 말것
  // https://stackoverflow.com/questions/65275188/how-to-use-the-react-hook-usememo-with-asynchronous-functions

 -> useEffect와 useState를 활용할 것
## 디버깅시
훅을 태스트하기 위해 디버깅으로 콘솔로그를 찍어볼 수 가 있는데
이때 두번 찍히는 이유는 `컴포넌트의 내용이 업데이트 되면 기존의 값을 언마운트하고 새로 값을 쓰니까 두번씩 찍히는 것