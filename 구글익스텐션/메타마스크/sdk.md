```ts
const mm = new MetaMaskSDK();
// init()을 호출하여 MetaMask를 초기화합니다.
void mm.init();
const provider = mm.getProvider();
const ethProvider = new ethers.BrowserProvider(provider);
```
