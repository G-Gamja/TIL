frame이란 inline frame의 약자입니다.

iframe 요소를 이용하면 해당 웹 페이지 안에 어떠한 제한 없이 또 다른 하나의 웹 페이지를 삽입할 수 있습니다.

문법

```
<iframe src="삽입할페이지주소"></iframe>
```

참조: https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage

# scroll 방지 & frameBorder없애기

iframe의 scroll, frameBorder props는 deprecated여서 css로 지정해주는 방식이 더 좋다

```css
width: 100%;
height: 100%;

position: relative;

// 스크롤 방지
overflow: hidden;

// frameBorder없애기
border: none;
```

참조: https://makersaid.com/how-to-prevent-scrolling-on-iframe/
