# Reduce

- Record 타입의 키 값을 동적으로 할당하는 방법
  => 변수 값을 템플릿 문자열로 사용하여 동적 속성 이름을 생성하려면 대괄호([])를 사용해야 합니다. 이를 통해 객체의 속성 이름을 동적으로 설정할 수 있습니다.

예시

```ts
const returnData = useMemo(
  () =>
    data?.reduce((acc: SimplePrice, item) => {
      acc[item.coinGeckoId] = {
        usd: item.current_price,
        usd_24h_change: item.daily_price_change_in_percent,
        usd_market_cap: item.market_cap,
      };
      return acc;
    }, {}) || undefined,
  [data, extensionStorage.currency]
);
```

에서 usd를 동적으로 할당하고 싶오

```ts
const returnData = useMemo(
  () =>
    data?.reduce((acc: SimplePrice, item) => {
      acc[item.coinGeckoId] = {
        [`${extensionStorage.currency}`]: item.current_price,
        [`${extensionStorage.currency}_24h_change`]:
          item.daily_price_change_in_percent,
        [`${extensionStorage.currency}_market_cap`]: item.market_cap,
      };
      return acc;
    }, {}) || undefined,
  [data, extensionStorage.currency]
);

export type SimplePrice = Record<
  string,
  {
    usd?: number;
    usd_24h_change?: number;
    usd_market_cap?: number;
    krw?: number;
    krw_24h_change?: number;
    krw_market_cap?: number;
    ...
  }
>;
```
