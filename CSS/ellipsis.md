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

한줄라인 글자수 제한
한줄 라인 글자수 를 제한하는 방법은 아래와 같습니다.
```
 <div class="txt_line">통영의 신흥보물 강구안의 동쪽벼랑인 동피랑의 벽화마을을 다녀왔다</div>
 .txt_line {
      width:70px;
      padding:0 5px;
      overflow:hidden;
      text-overflow:ellipsis;
      white-space:nowrap;
  }
```
Block레벨 테그에서만 적용됨.
overflow:hidden : 넓이가 70px를 넒어서는 내용에 대해서는 보이지 않게 처리함
text-overflow:ellipsis : 글자가 넓이 70px를 넘을 경우 생략부호를 표시함
white-space:nowrap : 공백문자가 있는 경우 줄바꿈하지 않고 한줄로 나오게 처리함 (\A로 줄바꿈가능)