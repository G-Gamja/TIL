https://stackoverflow.com/questions/48523118/wasm-module-compile-error-in-chrome-extension

에러 원문

```
libsodium-sumo.js:1 Uncaught (in promise) RuntimeError: Aborted(CompileError: WebAssembly.instantiate(): Refused to compile or instantiate WebAssembly module because neither 'wasm-eval' nor 'unsafe-eval' is an allowed source of script in the following Content Security Policy directive: "script-src 'self'"). Build with -sASSERTIONS for more info.
```

대충 해석하면, wasm 모듈을 컴파일할 수 없다고 나온다. 이는 크롬 확장프로그램에서 wasm 모듈을 사용할 수 없기 때문이다. 이를 해결하기 위해서는 크롬 확장프로그램의 manifest.json 파일에 아래와 같은 코드를 추가해주면 된다.

```json
"content_security_policy": "script-src 'self' 'unsafe-eval'; object-src 'self'"
```

csp관련 오류들: https://eddori.tistory.com/2

콘텐츠 보안 정책 관련 컬럼: https://web.dev/articles/csp?hl=ko
