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

  ## 커스텀 훅 에러

미해결 상태
```typescript
로컬스토리지에서 가져온 데이터를 상태변수로 선언
const [userList, setUserList] = useState(fetchedData ? (JSON.parse(fetchedData) as UserDataSetModel[]) : []);


// 커스텀 훅
// setItem api사용시 인자로 copiedList가 아닌 userList상태 변수 자체를 넘기면 저장이 안됨
  const addMethod = (inputName: string) => {
    const userInputTextData = { name: inputName };
    const copiedList = [...userList];
    copiedList.push(userInputTextData);
    setUserList(copiedList);
    // console.log(userList) 하면 한개씩 밀려서 찍힘, 방금 만든 카피리스트의 복사본이 아님
    window.localStorage.setItem('account', JSON.stringify(copiedList));//요기
  };
  ```

  콘솔 찍을떄 한개씩 밀리는데... 상태변수를 바꾸는 함수가 비동기적인가?


 <IconButton onClick={dialogg.onClose}>
  <Close />
</IconButton>
이거는 동작 안하고

// 부모로 부터 받은 dialogg.onClose함수
 <IconButton onClick={onCancel}>
  <Close />
</IconButton>
얘는 동작함 뭐임?