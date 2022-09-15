

### 자기 자신에게 값을 부여하는것은 이렇게 값을 부여해도 됨
```css
.footerSummitButton {
  box-sizing: border-box;
  width: 100%;
  &:disabled {
    background-color: #363c4d;
    color: #727e91;
    &:hover {
      cursor: no-drop;
    }
  }
}
```
### 폰트,텍스트 크기는 텍스트에게만 따로 먹이지 않아도 됨
```css
.footerSummitButton {
  box-sizing: border-box;
  width: 100%;
   color: white;
  font-family: 'Inter600';
  font-size: 1.5rem;
  line-height: 1.8rem;
  &:disabled {
    background-color: #363c4d;
    color: #727e91;
    &:hover {
      cursor: no-drop;
    }
  }
}
```