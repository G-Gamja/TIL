## map써서 컴포넌트 만들때 겪은 에러
https://crong-dev.tistory.com/47
```typescript
key={index}를 부여함으로써 개별 컴포넌트값이 키값을 가지도록 한다
근데 키는 index비추
  const userNameListView = userList.map((e, index) => (
    <AccountContainer key={index} mainText={e.name} onClickEdit={() => onClickEdit(e.name)} onClickDelete={() => onDelete(index)} />
  ));
  ```