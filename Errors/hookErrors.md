## 훅을 쓸떄 생길 수 있는 에러
두번째 인자로 넘기는 훅을 변경할 수 있는 함수에 넘기는 인자로 상태변수 그 자체를 넘기면 재렌더링이 일어나지 않음

=> 아마 컴포넌트의 상태값을 수정할때는 기존 값을 변경한느게 아니라 새로운 객체를 생성해야하기 떄문인 것 같다.
   * 리액트는 컴포넌트 상탯값을 불변객체로 관리한다. =>이를 통해 복잡도를 낮추고 버그 발생확률을 줄인다.
```typescript
const [userList, setUserList] = useState<userDataSetModel[]>(fetchedData ? (JSON.parse(fetchedData) as userDataSetModel[]) : []);

  const onDelete = (delNum: number) => {
    userList.map((e, index) => (index === delNum ? userList.splice(index, 1) : null));
    // setUserList에 상태값 그 잡체를 그대로 넘기면 내가 원하는대로 바로 반영안되는거였음
    const copiedList = [...userList];
    setUserList(copiedList);
    setUserList(userList);//에러!! 이러면 재렌더링X
    window.localStorage.setItem('account', JSON.stringify(userList));
  };
  ```

  ## useRef 훅 에러

  ### forwarded ref
  https://www.carlrippon.com/react-forwardref-typescript/

  https://ko.reactjs.org/docs/forwarding-refs.html