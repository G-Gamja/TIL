윈도우와 익스텐션의 통신을 위해서 postMessage 메서드를 사용한다. 

예를 들어, 아래 코드는 `iframe`이라는 아이디를 가진 `iframe` 으로 메시지를 보낸다.

그니까 postMessage를 호출한 놈(피동)한테 메시지를 쏜다는거임
```
var iframeWindow = document.getElementById(‘iframe’).contentWindow;
iframeWindow.postMessage(‘hello’, ‘http://ohgyun.com');
```
참고: https://ohgyun.com/532


단순 postMessage에 대한 설명
https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage



저삭-부모 간 post메시지 방법
https://itrooms.tistory.com/402


web to extension: stackoverFlow
https://stackoverflow.com/questions/11431337/sending-message-to-chrome-extension-from-a-web-page