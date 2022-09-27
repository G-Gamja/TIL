# 다이얼로그 만들기

https://agal.tistory.com/169

https://jeewonscript.tistory.com/25
# 보였다 안보였다 컴포넌트 만들기
1. 불리언 값을 props로 받아와서 이렇게 만들기

```
if (!visible) return null;
  return (
    <div className={styles.background}>
      <div className={styles.dialogContainer} ref={dialogRef}>
        <div className={styles.topHeader}>
```
2. 애초에 컴포넌트 부를때 불리언 값이랑 같이

# 다이얼로그 바깥 영역 클릭시 사라지게 하기 : out side click

* click은 마우스 버튼을 `눌렀다 놓았을 때` 시작합니다.  
* mousedown는 버튼을 `처음 누르는 순간` 시작됩니다.
# 리액트의 컴포넌트의 외부 영역의 클릭을 감지
https://sezzled.tistory.com/147  
https://chach4.tistory.com/4  

!유의할 점  
중요한 건 이때 사용자의 행동을 오직 다이얼로그에만 가능하게 하도록 하는 것이 중요하기 때문에 다이얼로그 이외의 모든 페이지 영역이 외부클릭으로 인식할 수 있도록 한다.
## 이벤트 버블링과 버튼 바깥 영역 클릭시 사라지게 하기
https://velog.io/@miyoni/TIL37 // 개념 정리가 잘 되어있음
 
https://curryyou.tistory.com/493 // 이것을 주로 참고함

event.target과 event.currentTarget(=this)의 차이점
event.target은 실제 이벤트가 시작된 타깃 요소이다. 버블링이 진행되어도 변하지 않는다.
event.currentTarget(=this)는 현재 요소로, 현재 실행중인 핸들러가 할당된 요소를 참조한다.
큰 틀 중에서도 가장 안쪽에 있는 요소가 event.target라면, 이 요소를 감싸는 큰 틀이 event.currentTarget(=this)라고 할 수 있겠다.

## 내가 겪었던 에러들

나랑 비슷, 대부분 해결됨
https://close-up.tistory.com/entry/React-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%ED%8A%B9%EC%A0%95-%EC%98%81%EC%97%AD-%EC%99%B8-%ED%81%B4%EB%A6%AD-%EA%B0%90%EC%A7%80

https://velog.io/@ptcookie78/TypeScript-React.js%EC%97%90%EC%84%9C-useRef-Hook-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0
```typescript
  // 다이얼로그 가시성 컨트롤
  const [dialogState, setDialogOpen] = useState(false);
  // 다이얼로그 컴포넌트에 접근을 위한 훅
  // 모달 외부 클릭시 끄기 처리
  // Modal 창을 useRef로 취득
  const dialogRef = useRef<HTMLDivElement>(null);
  // 이벤트 등록,제거 함수
  useEffect(() => {
    // 이벤트 핸들러 함수
    const handler = (event: React.BaseSyntheticEvent | MouseEvent) => {
      // mousedown 이벤트가 발생한 영역이 모달창이 아닐 때, 모달창 제거 처리
      if (dialogRef.current && !dialogRef.current.contains(event.target)) {
        setDialogOpen(false);
      }
    };
    // 여기서 에러가 발생
    // 이벤트 핸들러 등록
    document.addEventListener('mousedown', handler);
    return () => {
      // 이벤트 핸들러 해제
      document.removeEventListener('mousedown', handler);
    };
  }, [dialogRef]);
```

부모에서 자식 컴포넌트의 ref를 받아낼 때  
https://www.carlrippon.com/react-forwardref-typescript/  