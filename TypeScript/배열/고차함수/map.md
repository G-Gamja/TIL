# map 매서드 안에서 조건문 사용하기

```typescript
const a = sortedCurrentActivities.map((activity) => {
  const itemBaseAmount = activity.amount?.[0].amount || "0";

  const itemBaseDenom = activity.amount?.[0].denom || "";

  const assetCoinInfo = allTokens.find(
    (token) => token.address === activity.amount?.[0].denom
  );

  // 이렇게 사용하면 조건문이 많을 때 가독성 있게 코드를 짤 수 있다
  const decimals = (() => {
    if (itemBaseDenom === EVM_NATIVE_TOKEN_ADDRESS) {
      return currentEthereumNetwork.decimals;
    }

    if (assetCoinInfo?.decimals) {
      return assetCoinInfo.decimals;
    }

    return undefined;
  })();

  const itemDisplayAmount = (() => {
    if (decimals) {
      return toDisplayDenomAmount(itemBaseAmount, decimals);
    }

    return "0";
  })();

  const itemDisplayDenom = (() => {
    if (itemBaseDenom === EVM_NATIVE_TOKEN_ADDRESS) {
      return currentEthereumNetwork.displayDenom;
    }

    if (assetCoinInfo?.displayDenom) {
      return assetCoinInfo.displayDenom;
    }

    return itemBaseDenom.length > 5
      ? `${itemBaseDenom.substring(0, 5)}...`
      : itemBaseDenom;
  })();

  return (
    <ActivityItem
      key={activity.txHash}
      activity={activity}
      displayAmount={itemDisplayAmount}
      displayDenom={itemDisplayDenom}
      decimals={decimals}
    />
  );
});
```
