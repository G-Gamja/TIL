# What is Dom?
https://dev-cini.tistory.com/10

# 브라우저가 어떻게 띄워지나.. 그리고 가상Dom의 의의
https://junilhwang.github.io/TIL/Javascript/Design/Vanilla-JS-Virtual-DOM/#_1-%E1%84%87%E1%85%B3%E1%84%85%E1%85%A1%E1%84%8B%E1%85%AE%E1%84%8C%E1%85%A5-%E1%84%85%E1%85%A9%E1%84%83%E1%85%B5%E1%86%BC-%E1%84%80%E1%85%AA%E1%84%8C%E1%85%A5%E1%86%BC


## 실제 Dom의 값에 접근하는 방법

useRef훅을 사용해 ref가 붙은 돔 객체의 css값이라던지 다양한
값을 활용해 사용할 수 있다. 
유동적인 ui구성에 활용될 수 있다.

```jsx
export default function BaseLayout({ children, useHeader, useTitle }: BaseLayoutProps) {
    // useRef를 사용해 값을 가져옴
  const titleAreaRef = useRef<HTMLDivElement>(null);

  const [titleAreaHeight, setTitleAreaHeight] = useState(0);

  const containerHeight = `${60 - (useHeader ? 7.6 : 0) - titleAreaHeight / 10}rem`;

  useEffect(() => {
    setTitleAreaHeight(titleAreaRef.current?.clientHeight || 0);
  }, []);
  return (
    <Body>
      {useHeader && <Header {...useHeader} />}
      {useTitle && (
        <TitleAreaContainer ref={titleAreaRef}>
          <TitleContainer>
            <Typography variant="h2">{useTitle.title}</Typography>
          </TitleContainer>
          {useTitle.description && (
            <DescriptionContainer>
              <Typography variant="h4">{useTitle.description}</Typography>
            </DescriptionContainer>
          )}
        </TitleAreaContainer>
      )}
      <Container data-height={containerHeight}>{children}</Container>
    </Body>
  );
}
```
# 무한 스크롤 (Intersection Observer)
https://velog.io/@hyesungoh/%EB%82%B4%EA%B0%80-Intersection-Observer-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95