ellipsis적용한 div 아래에
바로 아래에 Typography를 배치해야 적용이 됨
```tsx

    <SwapCoinAmountContainer>
      <Typography variant="h5n">≈ {outputDisplayAmount}</Typography>
    </SwapCoinAmountContainer>

export const SwapCoinAmountContainer = styled('div')({
  display: 'flex',

  // 텍스트가 공간 넘길시 다음 줄로 안넘기게 하도록
  whiteSpace: 'nowrap',
  // 텍스트의 공간을 제한 시킴, Number 컴포넌트가 span이어서 텍스트의 크기많큼 늘어나서 최대를 제한함
  maxWidth: '6.5rem',
  '& > *': {
    // overflow시 가림처리
    overflow: 'hidden',
    textOverflow: 'ellipsis',
  },
});
```