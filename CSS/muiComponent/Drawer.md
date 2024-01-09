# 전체 화면을 기준이 아닌 특정 컴포넌트를 기점으로 Drawer를 열고 싶을 때

![바텀시트](/images/bottomsheet.png)

```tsx
<ValidatorBottomSheet
  open={isOpenedValidatorBottomSheet}
  onClose={() => setIsOpenedValidatorBottomsheet(false)}
  // 이걸 쓰면 특정 div를 기준으로 Drawer가 열림
  variant="persistent"
  onClickValidator={(validator) => {
    setCurrentValidator(validator);
    setIsOpenedValidatorBottomsheet(false);
  }}
/>;

// 스타일
export const ValidatorBottomSheet = styled(BottomSheet)({
  "& .MuiPaper-root": {
    height: "60rem",
    position: "absolute",
    bottom: "0",
  },
});

// 그리고 바텀시트 부모 컴포넌트에 position: relative를 줘야함
```

이러면 완성!
