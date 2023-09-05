# useState사용 시 주의할 점

처음 내 생각: `currentHomeTabIndex`를 useMemo로 선언했으니 의존성에 걸린 변수들이 변하면 다시 계산되어 useState의 초기값으로 들어가겠지?
실제: useMemo로 선언한 변수가 변해도 useState의 초기값은 변하지 않는다.
왜? -> 페이지간 이동이 없기에 페이지가 다시 그려지지 않아서 사실상 useState의 초깃값이 유지되어 에러 시 디폴트 값으로 전환이 되는 로직을 타지 않기 떄문

```ts
const currentHomeTabIndex = useMemo(
  () => (gte(currentTabIndex, tabLabels.length) ? 0 : currentTabIndex),
  [currentTabIndex, tabLabels.length]
);

// 한번 설정된 초깃값이 페이지변환이 이루어지지 않는다면 그대로 유지된다.
const [tabValue, setTabValue] = useState(currentHomeTabIndex);
```

따라서 useEffect를 사용해서 초기값을 설정(디폴트 로직이 들어간)해줘야 한다.

```ts
const [tabValue, setTabValue] = useState(
  tabLabels.map((item) => item.value).includes(currentTabIndex)
    ? currentTabIndex
    : 0
);

useEffect(() => {
  if (!tabLabels.map((item) => item.value).includes(currentTabIndex)) {
    setTabValue(0);
  }
}, [currentTabIndex, tabLabels]);
```
