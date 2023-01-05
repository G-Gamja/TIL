
- Cannot read properties of undefined ( reading 'map' )
 -> 리액트는 렌더링이 화면에 커밋 된 후에야 모든 효과를 실행하기 때문이다. 즉 React는 return에서 articles.map(...)을 반복실행할 때 첫 턴에 데이터가 아직 안들어와도 렌더링이 실행되며 당연히 그 데이터는 undefined로 정의되어 오류가 나는 것이다.
 -> map돌리는 값이 undefined되어있을때 이게 오링이 나는것