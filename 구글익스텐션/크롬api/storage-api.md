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

## session 스토리지와 local스토리지의 차이

session은 메모리에 local은 디스크에 저장이 된다.
그래서 브라우저가 꺼지거나 삭제된다면 세션 스토리지는 리셋됨

https://techblog.woowahan.com/5900/

# 스토리지 api공식 독스

https://developer.chrome.com/docs/extensions/reference/storage/
