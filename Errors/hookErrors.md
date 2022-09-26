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
      // TODO 현재 커스텀훅에서 직접 onClose,onOpen 실행시키면 작동을 안함
    // 아마 다른 컴포넌트에서 다른 상태변수를 작동시켜서 부모에 있는 상태 변수를
    // 조작해서 껏다 켰다 해야하는데 이름만 같은 자식의 상태변수를 조작하면 작동 안걸림
    // forwardedref를 사용해서 해결해보자

    문제상황: 부모 , 자식 따로 커스텀훅을 발동시키고 커스텀훅을 통해서 자식의
    disable여부를 결정함 근데 자식에서 선언한 커스텀훅을 통해서는 on/off가 안됨
    => 커스텀 훅은 독립적으로 선언되기 때문, 즉 부모에서 선언한 상태변수랑 
    자식에서 선언한 자식의 커스텀 훅의 상태변수랑 다른거임 자식의 커스텀훅을 통해 상태변수 업데이트를 해도 그건 자식의 상태변수를 수정한거기때문에 부모에서 선언해서 부모에서 사용중인 상태변수는 업데이트가 안된것임
    => 그냥 부모에서 자식에게로 커스텀 훅의 함수를 넘기자  

    문제상황2: 마찬가지로 자식, 부모의 상태변수가 다르기 때문에 useRef또한 서로 다른 것+상태변수가 서로 다름의 연동으로 다이얼로그 외부를 클릭해도 안나가짐
    ==> 부모에서 선언한 커스텀 훅의 ref값을 forwardRef로 자식에게 전달해서 자식의 레퍼런스를 받아서 부모에서 외부클릭 여부를 처리하자
``` typescript 
부모에서 선언된 상태변수를 통해 자식 컴포넌트의 visible을 컨트롤 하는 모습
<부모>

 {dialog.isDialog && (
        <Dialog mainText="Rename account" accountName={user.editData.nameVal} onCancel={dialog.onClose} callback={user.editMethod} ref={dialog.dialogRef} />
      )}
      <부모>
```

```typescript 
선언형 함수의 forwardRef 사용법
function Dialog({ mainText, accountName, onCancel, callback }: DialogProps, ref: React.LegacyRef<HTMLDivElement> | undefined) {
  const userInput = useInput();
  const dialogg = useDialog();

  const onSubmit = () => {
    callback(userInput.userName);
    onCancel();
  };
  return (
    <div className={styles.background}>
      <div className={styles.dialogContainer} ref={ref}>
        <div className={styles.topHeader}>
          {mainText}
          <div className={styles.iconButton}>
            <IconButton onClick={onCancel}>
              <Close />
            </IconButton>
          </div>
        </div>
        <div className={styles.mainContainer}>
          <input type="text" className={styles.inputBox} value={userInput.userName} onChange={userInput.onChange} placeholder={accountName} />
          <button type="button" className={styles.footerSummitButton} disabled={userInput.userName === ''} onClick={onSubmit}>
            Submit
          </button>
        </div>
      </div>
    </div>
  );
}
const DDialog = React.forwardRef(Dialog);
export default DDialog;

```

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