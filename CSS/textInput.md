# text input 관련 css 팁들

- 오른쪽부터 타이핑을 시작하고 싶어요
    - mui
    ```css
    '&.MuiOutlinedInput-root': {
    '.MuiOutlinedInput-input': {
      textAlign: 'right',
        }
    }
    ```
- prefix를 넣고 싶어요

```jsx
// 아래의 속성 넣어주기
endAdornment={
              <InputAdornment position="end">
                <MaxButton
                  type="button"
                  onClick={() => {
                    setCurrentDisplayAmount(maxDisplayAmount);
                  }}
                >
                  <Typography variant="h7">MAX</Typography>
                </MaxButton>
              </InputAdornment>
            }

```
adornment css 조작
```css
'& .MuiInputAdornment-root': {
    backgroundColor: theme.palette.divider,
    padding: '30px 1px',
    margin: '0',
    borderTopLeftRadius: theme.shape.borderRadius + 'px',
    borderBottomLeftRadius: theme.shape.borderRadius + 'px',
  },
```
참조: https://mui.com/material-ui/react-text-field/#input-adornments