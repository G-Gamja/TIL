styled안에 svg컴포넌트를 넣어줘
```typescript
export const TokensIcon = styled(Tokens)(({ theme }) => ({
  stroke: theme.colors.base05,
  marginBottom: '0.8rem',
}));
```
