ellipsis적용한 div 아래에
바로 아래에 Typography를 배치해야 적용이 됨

```tsx
<SwapCoinAmountContainer>
  <Typography variant="h5n">≈ {outputDisplayAmount}</Typography>
</SwapCoinAmountContainer>;

export const SwapCoinAmountContainer = styled("div")({
  display: "flex",

  // 텍스트가 공간 넘길시 다음 줄로 안넘기게 하도록
  whiteSpace: "nowrap",
  // 텍스트의 공간을 제한 시킴, Number 컴포넌트가 span이어서 텍스트의 크기많큼 늘어나서 최대를 제한함
  maxWidth: "6.5rem",
  "& > *": {
    // overflow시 가림처리
    overflow: "hidden",
    textOverflow: "ellipsis",
  },
});
```

네, Mui의 Typography 컴포넌트에서 variant prop 값을 변경하면 text ellipsis가 적용되지 않을 수 있습니다.

예를 들어, variant="body1" 으로 설정된 Typography에서는 기본적으로 텍스트 줄 바꿈이 허용됩니다. 이 경우, 텍스트가 너무 길어서 한 줄에 모두 표시되지 않을 때 자동으로 다음 줄로 내려가게 됩니다. 이러한 경우에는 text ellipsis가 적용되지 않으므로, 텍스트가 너무 길어지면 요소의 크기가 늘어나게 됩니다.

다른 측면에서, variant="subtitle1"과 같은 값으로 설정된 Typography 에서는 text ellipsis가 적용됩니다. 이 경우, 긴 텍스트가 있는 경우 일정 길이 이상의 문자가 잘립니다. 그리고 마침표 (...) 가 엘립시스 부분을 대체합니다.

해결책으로는 Typography 컴포넌트와 함께 noWrap prop을 사용하여 text ellipsis를 항상 적용할 수 있습니다. 또는, CSS 속성 white-space: nowrap; overflow: hidden; text-overflow: ellipsis; 를 직접 적용해서 수동으로 처리하는 방법도 있습니다.

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

```ts
export const ObjectDescriptionTextContainer = styled("div")(({ theme }) => ({
  color: theme.colors.text01,

  // 부모 컴포넌트에서 width를 정해줘야함
  maxWidth: "100%",

  wordBreak: "keep-all",
  whiteSpace: "nowrap",

  "& > *": {
    overflow: "hidden",
    textOverflow: "ellipsis",
  },
}));
```

child 간격 고려해서 반응형으로 말줄임 관련해서 읽어볼만한: https://taegon.kim/archives/10549

# 여러줄인 텍스트 말줄임

```tsx
export const DescriptionContainer = styled("div")(({ theme }) => ({
  color: theme.colors.base02,

  // 맥스 줄 수
  WebkitLineClamp: 3,
  WebkitBoxOrient: "vertical",

  width: "100%",
  display: "-webkit-box",

  wordWrap: "break-word",

  overflow: "hidden",
}));
```
