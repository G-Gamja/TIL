윈도우에서는 아래와 같은 메서드를 통해 아래의 메서드를 호출하게 된다.
window.cosmostation.cosmos.request({})  
여기 파라미터안에 메시지의 타임에 따라 해당하는 타입의 리퀘스트가 걸린다.

1. 웹에서 리퀘스트 호출
    1. 응답 메시지 리스너를 add한다. 이는 추후 컨텐츠 스크립트가 웹으로 응답 메시지를 post했을때 이를 감지하기 위함이다
    2. 메시지를 컨텐츠 스크립트로 post한다
2. 컨텐츠 스크립트에서 add한 메시지 이벤트 리스너가 fire된다. 메시지의 타임이 webtocontent인 경우
    1. fire되면 핸들러가 호출되고 핸들러 안에서 크롬 api(메시지 패싱)을 통해 백그라운드 스크립트로 해당 메시지를 send한다
3. 백그라운드 스크립트에서 onMessage.addListener를 통해 컨텐츠에서 들어오는 메시지를 받는다, 이때 메시지의 타입에 따라 cstob메서드를 작성시키는데...
    1.cstob메서드 에서는 컨텐츠 스크립트로 보낼 응답 메시지를 만들어 tabs.sendMessgae를 통해 응답 메시지를 전달한다
4. 콘텐츠 스크립트의 onMessage .addListener에서 응답 메시지 즉 backtocontent를 받고 핸들러가 fire된다
5.핸들러는 웹으로 응답 메시지를 post한다
6. 1 과정에서 등록한 MESSAGE_TYPE.RESPONSE__WEB_TO_CONTENT_SCRIPT 메시지 이벤트가 리스너가 fire되고 해당 리스너에 등록한 핸들러가 작동하고 결과에 따라 리퀘스트 작업이 종료 된다.
```typescript
request: (message: EthereumRequestMessage) =>
        new Promise((res, rej) => {
          const messageId = uuidv4();

          // 요청하고 받는 
          // 컨텐츠에서 윈도우로 postMessage해서 fire된 메시지의 메시지의 id와 일치하면 res,rej실행 후
          const handler = (event: MessageEvent<ContentScriptToWebEventMessage<ResponseMessage, EthereumRequestMessage>>) => {
            if (event.data?.isCosmostation && event.data?.type === MESSAGE_TYPE.RESPONSE__WEB_TO_CONTENT_SCRIPT && event.data?.messageId === messageId) {
              window.removeEventListener('message', handler);

              const { data } = event;

              if (data.response?.error) {
                rej(data.response.error);
              } else {
                res(data.response.result);
              }
            }
          };

          window.addEventListener('message', handler);

          // 요청
          window.postMessage({
            isCosmostation: true,
            line: LINE_TYPE.ETHEREUM,
            type: MESSAGE_TYPE.REQUEST__WEB_TO_CONTENT_SCRIPT,
            messageId,
            message,
          });
        }),
```