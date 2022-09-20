## 컴포넌트화 하는법


``` typescript
type MainContainerProps = {
  mainText: string;
  onClick?: () => void;
};
export default function MainContainer({ mainText, onClick }: MainContainerProps) {
  return (
    <button type="button" className={styles.buttonStyle} onClick={onClick}>
      <div className={styles.boxButton}>
        <div className={styles.boxText}>{mainText}</div>
        <div className={styles.boxIcon}>
          <RightStroke />
        </div>
      </div>
    </button>
  );
}
```

## 컴포넌트 여러개 반복 렌더링 하는법 

https://goddaehee.tistory.com/303

## 에러
* 컴포넌트에 props전달시 생긴 에러(해결)
=> useState를 여러개 쓸 때 사용해야 하는 문법을 놓침: https://velog.io/@unknown9732/useState-%EC%97%AC%EB%9F%AC%EA%B0%9C%EB%A5%BC-%EC%93%B0%EB%8A%94-%EA%B2%BD%EC%9A%B0
```  
const [editData, setEditData] = useState({ name: '', index: 0 });
//accountName이거에 전달되는 값이 안넘어가나봐
{dialogState && <Dialog mainText="Rename account" accountName={editData.name} onCancel={onCancel} callback={getUserInputDataFunc} />}
```
상태변수 바꾸는 함수는 잘 작동하는데 플레이스 홀더 즉 스트링값이 안넘어간다  직접값 참조에 있어서 문제가 있는 것으로 보임