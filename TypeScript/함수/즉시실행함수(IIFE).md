# 즉시 실행 함수 (IIFE)

이 코드는 pubKey.type의 값에 따라 typeURL 값을 결정하는 즉시 실행 함수 표현식(IIFE, Immediately Invoked Function Expression)입니다.

pubKey.type의 값이 'ethermint/PubKeyEthSecp256k1'인 경우, typeURL은 '/ethermint.crypto.v1.ethsecp256k1.PubKey'가 됩니다.

pubKey.type의 값이 'injective/PubKeyEthSecp256k1'인 경우, typeURL은 '/injective.crypto.v1beta1.ethsecp256k1.PubKey'가 됩니다.

위의 두 경우가 아닌 경우, typeURL은 '/cosmos.crypto.secp256k1.PubKey'가 됩니다.

즉시 실행 함수 표현식(IIFE)은 함수를 선언함과 동시에 실행하는 JavaScript의 패턴입니다. 이 패턴은 주로 임시적인 스코프를 생성하여 변수의 범위를 제한하는 데 사용됩니다. 이 경우에는 typeURL 값을 조건에 따라 결정하는 데 사용되었습니다.

```ts
const typeURL = (() => {
  if (pubKey.type === "ethermint/PubKeyEthSecp256k1") {
    return "/ethermint.crypto.v1.ethsecp256k1.PubKey";
  }

  if (pubKey.type === "injective/PubKeyEthSecp256k1") {
    return "/injective.crypto.v1beta1.ethsecp256k1.PubKey";
  }

  return "/cosmos.crypto.secp256k1.PubKey";
})();
```

# 즉시실행 함수 (IIFE, Immediately Invoked Function Expression)

즉시실행함수 (IIFE, Immediately Invoked Function Expression)는 정의되자마자 즉시 실행되는 함수를 말한다.

즉시실행함수는 다음과 같이 소괄호(())로 함수를 감싸서 실행하는 문법을 사용한다.

```javascript
(function () {
  console.log("IIFE");
})();

// 화살표 함수로도 사용 가능하다
(() => {
  console.log("IIFE");
})();
```

자바스크립트 콘솔에 위 함수를 찍어보면 선언과 동시에 IIFE를 출력함을 확인 할 수 있다.

참조: https://jongminfire.dev/java-script-%EC%A6%89%EC%8B%9C%EC%8B%A4%ED%96%89%ED%95%A8%EC%88%98-iife

# 함수형 변수

IIFE를 활용하여 함수형 변수를 만들 수 있다.

```typescript
// 함수 로 나온값을 바로 호출해서 변수에 할당하는 방식이다
// FIXME later/... useMemo로 바꿔보기
const itemDisplayAmount = // 여기가 익명함수
  (() => {
    if (itemBaseDenom === baseDenom) {
      return toDisplayDenomAmount(itemBaseAmount, decimals);
    }

    if (assetCoinInfo?.decimals) {
      return toDisplayDenomAmount(itemBaseAmount, assetCoinInfo.decimals);
    }

    if (ibcCoinInfo?.decimals) {
      return toDisplayDenomAmount(itemBaseAmount, ibcCoinInfo.decimals);
    }

    if (chain.baseDenom === baseDenom) {
      return toDisplayDenomAmount(itemBaseAmount, chain.decimals);
    }

    return itemBaseAmount || "0";
  })();
// 함수 선언 하고 바로 호출해서 그 값을 변수에 할당하는 방식
```
