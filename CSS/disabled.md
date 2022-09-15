# disabled 요소 한번에 코드짜기

https://developer.mozilla.org/ko/docs/Web/CSS/:disabled

버튼요소중에서 disabled인것
```css
button[type='button']:disabled {
  &:hover {
    cursor: no-drop;
  }
}

```