# 부모 자식간 props전달하기

https://technicolour.tistory.com/56


# 팁 1 굳이 자식으로 넘겨주는 객체의 타입과 자식에서 선언한 props의 타입과 완벽히 동일할 필요가 없다

부모-> 자식 으로 넘기는 데이터의 타입이 자식 props 선언한 값의 타입보다 더 범위가 포괄적이면
굳이 부모에서 값을 재단할 필요없이 넘겨도 괜찮음

예시 
``` html
currentFeeCoin이 selectedFeeCoin보다 더 포괄적인 타입임
Fee에서 원하는 타입과 정확히 일치하는 타입은 selectedFeeCoin이다
 <Fee
     feeCoin={{ ...currentFeeCoin}}
    />
 <Fee
    feeCoin={selectedFeeCoin}
    />
```
# props로 jsx컴포넌트 넘기기

흔히 아는 이 구조가 그런 구조인데
```jsx
 <ManageButton>
        <Sample />
    </ManageButton>
```
그럴려면 부모에서 children을 아래와 같이 선언해줘야함
```tsx
type BaseLayoutProps = {
  useHeader?: React.ComponentProps<typeof Header>;
  useSubHeader?: {
    title: string;
    onClick?: () => void;
  };
  // JSX타입
  children: JSX.Element;
};

export default function BaseLayout({ useHeader, useSubHeader, children }: BaseLayoutProps) {
  const containerHeight = `${60 - (useHeader ? 5.2 : 0) - (useSubHeader ? 4.4 : 0)}rem`;

  return (
    <Body>
      {useHeader && <Header {...useHeader} />}
      {useSubHeader && <SubHeader {...useSubHeader} />}
      <Container data-height={containerHeight}>{children}</Container>
    </Body>
  );
}
```