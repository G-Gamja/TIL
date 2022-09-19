# 다이얼로그 만들기

https://agal.tistory.com/169

https://jeewonscript.tistory.com/25

# 다이얼로그 바깥 영역 클릭시 사라지게 하기 : out side click

https://chach4.tistory.com/4

## 이벤트 버블링과 버튼 바깥 영역 클릭시 사라지게 하기
https://velog.io/@miyoni/TIL37

event.target과 event.currentTarget(=this)의 차이점
event.target은 실제 이벤트가 시작된 타깃 요소이다. 버블링이 진행되어도 변하지 않는다.
event.currentTarget(=this)는 현재 요소로, 현재 실행중인 핸들러가 할당된 요소를 참조한다.
큰 틀 중에서도 가장 안쪽에 있는 요소가 event.target라면, 이 요소를 감싸는 큰 틀이 event.currentTarget(=this)라고 할 수 있겠다.