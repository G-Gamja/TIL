# 왜 함수형 컴포넌트를 사용하는가
- 함수형 컴포넌트는 jsx를 바로 반환하는 반면 클래스는 render()메서로 변환 후 반환해서 번거로움
  - 상속이 필요없어 코드가 간결
- 바벨로 컴파일시 더 가벼운 코드를 생성
- this 불필요
- 
## 컴포넌트화 하는법

---------
### https://velog.io/@donggu/%EB%AC%B8%EA%B3%BC%EC%83%9D%EC%9D%B4-%EC%84%A4%EB%AA%85%ED%95%98%EB%8A%94-React-propsproperties-children  
다양한 조건에 따라 컴포넌트 만드는 법


### https://ko.reactjs.org/docs/jsx-in-depth.html  쿨북
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
--------
## 컴포넌트에 다른 컴포넌트 집어넣기: `children` 사용
```typescript

function App() {
  return (
    <House>//컴포넌트
      <First />//컴포넌트 내 타 컴포넌트
      <Second name="동2" color="blue" />
    </House>
  );
}


function House({ children }) {
  const style = {
    border: '4px solid green',
    padding: '16px',
  };
  return <div style={style}>{children}</div>;
}

export default House;
```

* `props`: 어떤 컴포넌트를 import해와서 사용하는 부모(상위) 컴포넌트 (ex. App.js)에서 정하는 값입니다. `부모 컴포넌트에서 설정해서 자식 컴포넌트로 전달하여, 자식 컴포넌트에서 쓰입니다`.
  * 아래 컴포넌트에 mainText등 스트링 값을 넘겨줄때
* `children`: `A` 컴포넌트 사이에 `B` 컴포넌트가 있을 때, A 컴포넌트에서 B 컴포넌트 내용을 보여주려고 사용하는 props입니다.
  * 아래 컴포넌트에 SVG컴포넌트를 넘겨주고 싶을때 
-----
## Props의 기본값은 “True”
Prop에 어떤 값도 넘기지 않을 경우, 기본값은 true입니다. 아래의 두 JSX 표현은 동일한 표현입니다.
```typescript

<MyTextBox autocomplete />

<MyTextBox autocomplete={true} />
```
## 속성 펼치기
props에 해당하는 객체를 이미 가지고 있다면,...를 “전개” 연산자로 사용해 전체 객체를 그대로 넘겨줄 수 있습니다. 아래의 두 컴포넌트는 동일합니다.
```typescript
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```
컴포넌트가 사용하게 될 특정 prop을 선택하고 나머지 prop은 전개 연산자를 통해 넘길 수 있습니다.
```
const Button = props => {
  const { kind, ...other } = props;
  const className = kind === "primary" ? "PrimaryButton" : "SecondaryButton";
  return <button className={className} {...other} />;
};

const App = () => {
  return (
    <div>
      <Button kind="primary" onClick={() => console.log("clicked!")}>
        Hello World!
      </Button>
    </div>
  );
};
```
위의 예시의 kind prop은 소비되고 DOM의 button element에 넘겨지지 않습니다. 다른 모든 prop은 ...other 객체를 통해서 넘겨지며 이 컴포넌트를 유연하게 만들어줍니다. onClick과 children prop으로 넘겨지는 것을 볼 수 있습니다.

전개 연산자는 유용하지만 불필요한 prop을 컴포넌트에 넘기거나 유효하지 않은 HTML 속성들을 DOM에 넘기기도 합니다. 꼭 필요할 때만 사용하는 것을 권장합니다.



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