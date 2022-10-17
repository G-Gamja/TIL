https://velog.io/@devjuun_s/Chrome.storage-%EC%A0%95%EB%A6%AC

## 스토리지 업데이트에 대한 동기적 반응

`storage.onChanged`
```javascript
chrome.storage.onChanged.addListener(function (changes, namespace) {
  for (let [key, { oldValue, newValue }] of Object.entries(changes)) {
    console.log(
      `Storage key "${key}" in namespace "${namespace}" changed.`,
      `Old value was "${oldValue}", new value is "${newValue}".`
    );
  }
});
```