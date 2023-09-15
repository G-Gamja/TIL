```ts
export const StyledButton = styled("button")({
  border: "1px solid transparent",
  // 첫번째는 버튼 배경색, 두번째는 버튼 테두리 색
  background:
    "linear-gradient(#1F1F2C, #1F1F2C), linear-gradient(90deg, #9C6CFF 37.5%, #05D2DD 100%)",
  backgroundOrigin: "border-box",
  backgroundClip: "content-box, border-box",

  width: "fit-content",

  borderRadius: "1rem",

  color: "white",

  padding: "0",

  "&:hover": {
    opacity: "0.8",
  },

  cursor: "pointer",
});
```

https://velog.io/@dusunax/CSS-border%EC%97%90-gradient-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0
