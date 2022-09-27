# typescript 작성 시의 규칙

## 컴포넌트 props명
이름을 컴포넌트 이름 뒤에 Props 라고 붙여주는 것을 규칙으로 합시다.
-> type MainContainerProps
## 함수를 사용할 때 인라인, 새로 감싸는 함수중 하나를 택할 것  
```html
//둘의 방식 중 하나를 택할 것
<MainContainer mainText="Create a new account" onClick={() => navigate('/account')} />
<MainContainer mainText="Create a new account" onClick={() => goAccount()} />

```
## 타입 선언시
type userDataSetModel = { name: string };

는 함수 밖에서 선언할 것, 그리고 타입은 대문자로 시작

## map,filter  쓸때 안쓰는 인자 처리
e대신 _로 바꿀것  

```typescript
const filteredList = userList.filter((e, index) => index !== selectedindex);

여기서 인자로 넘기는 e대신 _로 바꾸기

const filteredList = userList.filter((_, index) => index !== selectedindex);
```
## 상태변수의 업데이트는 무조건 set함수를 통해서만

    const filteredList = userList.filter((_, index) => index !== selectedindex);
    // 수정 후
      const copiedList = [...userList];
    const filteredList = copiedList.filter((_, index) => index !== selectedindex);
    setUserList(filteredList);
여기서 유저리스트는 상태변수인데 필터함수를 통해
상태변수가 직접 조작이 된다. 따라서 배열 깊은 복사를 통해 복사본을 조작한 후 그 복사본을 set함수를 써서 상태변수와 같게 만들어주자


## 선택된 배열의 데이터를 관리하는 법
```
const [editData, setEditData] = useState({ nameVal: '', indexVal: 0 });

<Dialog
          mainText="Rename account"
          accountName={editData.name}
          onCancel={dialog.onClose}
          callback={user.editMethod}
          selected={editData.indexVal}
          ref={dialog.dialogRef}
        />

  const [selectedidx, setEditData] = useState(0);

  <Dialog
          mainText="Rename account"
          accountName={user.userList[selectedidx].name}
          onCancel={dialog.onClose}
          callback={user.editMethod}
          selected={selectedidx}
          ref={dialog.dialogRef}
        />
```
기존에는 리스트뷰들의 요소중 선택됐다면 선택된 요소의 이름과 배열에서의 인덱스를 상태변수로 관리했다. 하지만 이 방식 보다는 해당 요소를 특정지을 수 있는 인덱스만을 저장해서 이름이 필요하다면 직접 인덱스값을 참조해서 사용하는 편이 코드 유지보수에 좋다

왜?? 지금은 이름,인덱스 뿐이지만 만약 다이얼로그에서 필요한 정보가 늘어난다면 그 것들을 모두 상태변수로 선언해줘야 하기 때문이다. 하지만 인덱스만 있다면 배열에서 직접 참조할 수 있기 때문에 이 방식이 더 유연하다고 볼 수 있다

## 반복되는 컴포넌트 그릴때
기존에는 리턴함수 외부에서 배열 컴포넌트를 선언한 후 내부에 집어넣었는데
이런 방식은 해당 컴포넌트 배열이 특징이 잘 되지 않기떄문에
수정 후와 같이 리턴함수 내부(element)에서 그대로 선언할 것
* 그리고 따지고 보면 요 컴포넌트 배열도 컴포넌트니까 이름 쓸때 
UserNameListView로 해야해...
```typescript
const userNameListView = user.userList.map((e, index) => (
     <AccountContainer key={index} mainText={e.name} onClickEdit={() => onClickEdit(index)} onClickDelete={() => user.deleteMethod(index)} />
   ));

   <div className={styles.ListView}>
            {userNameListView}
          </div>
 수정 후

     <div className={styles.ListView}>
            {user.userList.map((e, index) => (
              <AccountContainer key={index} mainText={e.name} onClickEdit={() => onClickEdit(index)} onClickDelete={() => user.deleteMethod(index)} />
            ))}
          </div>     
```

## 불필요한 요소는 전달하지 말 것 (콜백)
```
 <Dialog
          mainText="Rename account"
          accountName={user.userList[selectedidx].name}
          onCancel={dialog.onClose}
          callback={user.editMethod}
          selected={selectedidx}
          ref={dialog.dialogRef}
        />
콜백으로 넘기는 함수

  const editMethod = (inputEditName: string, selectedindex: number) => {
    const userInputTextData = { name: inputEditName };
    const copiedList = [...userList];
    copiedList.splice(selectedindex, 1, userInputTextData);
    setUserList(copiedList);
    window.localStorage.setItem('account', JSON.stringify(userList));
  };

  인데 굳이 여기서 selected를 자식 컴포넌트로 줄 필요가 없다. 자식에서 인덱스값을 display하는 것도 아니고 로직도 부모에서 콜백으로 넘기는거라 부모에서만 필요하니깐
```

수정 후
```
//자식 컴포넌트   
type DialogProps = {
  mainText: string;
  accountName: string;
  onCancel: () => void;
  callback: (name: string) => void;
};

function Dialog({ mainText, accountName, onCancel, callback }: DialogProps, ref?: React.ForwardedRef<HTMLDivElement>) {
  const userInput = useInput();
  const onSubmit = () => {
    callback(userInput.userName);
    onCancel();
  };  
//

//부모 선언부
   <Dialog
          mainText="Rename account"
          accountName={user.userList[selectedIdx].name}
          onCancel={dialog.onClose}
          //여기
          callback={(username) => user.editMethod(username, selectedIdx)}
          ref={dialog.dialogRef}
        />
```
콜백함수를 인자를 하나만 받는 익명함수로 만들면 자식에 굳이 불필요한 인자 안넘겨도 원하는 기능을 구현할 수 있는 것

원래는 edit함수 자체를 넘겨서 두개의 인자가 필요해서 억지로 필요없는 인덱스 인자도 자식으로 넘겨서 자식에서  edit메서드를 호출했는데 중간에 자식에게 edit자체를 넘기는게 아니라 익명함수를 만들어 인자를 받게 하면 굳이 인덱스 인자를 넘기지 않아도 된다