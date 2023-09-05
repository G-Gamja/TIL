탭 컴포넌트에서 탭의 순서로 index를 정하는게 아닌 내가 원하는 값으로 index를 정하고 싶을 때는 value를 사용한다.

```tsx
<Tabs value={tabValue} onChange={handleChange} variant="fullWidth">
  {tabLabels.map((item, i) => (
    <Tab key={item} label={item} value={item === "Activity" ? 10 : i} />
  ))}
</Tabs>
```
