그러나 아래와 같이 custom 한 스크립트를 동작시키고 싶을 때 원하는 동작이 실행되지 않을 수도 있습니다.


window.__myString = "퇴근하자";


이렇게 대입을 해도 클라이언트 사이드에서는 적용이 안됩니다. 😓


이런 경우에는 script 태그를 만들어서 DOM에 append 해주는 방식으로 처리해야 합니다.

```
const clickcrScriptEl = document.createElement("script");
clickcrScriptEl.innerHTML = `window.__myString = "퇴근하자";`;
document.body.appendChild(clickcrScriptEl);

```
하지만!! 이렇게 모두 script를 string으로 대입하는 것은.. 굉장히 번거로운 작업이져!!

```
var fullPath = chrome.extension.getURL('scripts/abc.js');

const script = document.createElement('script');
script.type = 'text/javascript';
script.src = fullPath;
document.body.appendChild(script);

```
chrome.extension.getURL을 사용하면 chrome extension 내 스크립트 환경에 있는 script 를 fullpath로 가져올 수 있습니다.

참고: https://tidyline.gitbook.io/today-i-learned/etc/chromeextension