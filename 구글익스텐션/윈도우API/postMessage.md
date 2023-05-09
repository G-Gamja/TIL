`window.addEventListener('message', handler)` 함수는 웹 API의 `Window.postMessage()`를 사용하여 다른 윈도우에게 메시지를 보낼 때 발생하는 이벤트입니다.

`postMessage()` 메소드를 사용하면, 다른 웹 사이트에서 우리 웹 사이트로 데이터를 전달할 수 있습니다. 이때 이 메시지를 받기 위해서는 `window.addEventListener()` 메소드를 사용하여 메시지를 처리할 콜백 함수를 등록해주어야 합니다.

예를 들어, 다음과 같은 코드가 있다고 가정해봅시다:

```js
// 다른 윈도우의 스크립트
let data = { message: "Hello from another window!" };
window.opener.postMessage(data, "*");
```

이 코드는 다른 윈도우에서 현재 윈도우에 대한 참조(`window.opener`)를 획득하고, `postMessage()` 메소드를 호출하여 데이터를 현재 윈도우로 전달합니다.

그 다음으로, 현재 윈도우에서 이 메시지를 처리하기 위해 다음과 같이 리스너를 등록할 수 있습니다:

```js
// 이 윈도우의 스크립트
function handleMessage(event) {
  if (event.origin !== "https://example.com") {
    // 보낸 윈도우의 도메인을 확인하기
    return;
  }
  console.log(event.data.message);
}

window.addEventListener("message", handleMessage, false);
```

이제 다른 윈도우에서 메시지를 보내면, 이 메시지를 처리하는 콜백 함수 `handleMessage()`가 실행됩니다. 이때 보낸 윈도우의 도메인이 특정 도메인으로 제한된다면, 다른 도메인에서 전달되는 악성 코드 등을 방지할 수 있습니다.
