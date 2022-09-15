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