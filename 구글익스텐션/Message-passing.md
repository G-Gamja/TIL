# messaging

공식 홈페이지: https://developer.chrome.com/docs/extensions/mv3/messaging/


우리는 직접 content.js파일에 접근할 수 없음. 그래서 우리는 Message Passing이라는 방식을 사용해야함


보내는쪽: sendMessage  

이렇게 메시지를 보내면 받는 쪽의 runtime.onMessage가 fire되고 

메서드에 대한 설명과 파라미터들에 대한 설명
https://developer.chrome.com/docs/extensions/reference/runtime/#method-sendMessage
```javascript
chrome.runtime.sendMessage(
    // 메시지를 보낼 확장 프로그램/앱의 ID
    // 생략 시 메시지는 본인(익스텐션)으로 간다
  extensionId?: string,
  message: any,
  options?: object,
  callback?: function,
)
```
받는 쪽: runtime.onMessage 이벤트 리스너로 들어온 메시지를 핸들링할 수 있다.