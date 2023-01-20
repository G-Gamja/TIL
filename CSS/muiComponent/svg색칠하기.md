이렇게 스타일드 안에 svg컴포넌트 집어넣고 fill이나 stroke로 색칠해주면댐
```css
export const StyledSearch20Icon = styled(Search20Icon)(({ theme }) => ({
  fill: theme.colors.base05,
}));
```
