## 배열

## 배열 삭제
https://developer-talk.tistory.com/153
## 배열복사
https://yangheat.tistory.com/54

## filter method

조건에 따라 배열의 값들을 걸러주는 함수이다

map은 배열을 재가공한다는 느낌이기 때문에 단순히 splice등 배열 내의 특정 요소의 삭제등을 위해선 filter메서드가 더 합리적인 선택이다
```typescript
 const deleteMethod = (selectedindex: number) => {
    // userList.map((e, index) => (index === selectedindex ? userList.splice(index, 1) : null));

    덤으로 불필요한 계산 과정도 줄일 수 있다.
    // const copiedList = [...userList]
    const copiedList = userList.filter((e, index) => index !== selectedindex);
    setUserList(copiedList);
    window.localStorage.setItem('account', JSON.stringify(userList));
  };
  ```