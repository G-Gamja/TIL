## 배열

## 배열 삭제

https://developer-talk.tistory.com/153

## 배열복사

https://yangheat.tistory.com/54

## filter method

조건에 따라 배열의 값들을 걸러주는 함수이다

map은 배열을 재가공한다는 느낌이기 때문에 단순히 splice등 배열 내의 특정 요소의 삭제등을 위해선 filter메서드가 더 합리적인 선택이다

```typescript
 const deleteMethod = (selectedindex: number) => {
    // userList.map((e, index) => (index === selectedindex ? userList.splice(index, 1) : null));

    덤으로 불필요한 계산 과정도 줄일 수 있다.
    // const copiedList = [...userList]
    const copiedList = userList.filter((e, index) => index !== selectedindex);
    setUserList(copiedList);
    window.localStorage.setItem('account', JSON.stringify(userList));
  };
```

# 배열을 string으로 변환

`arr.join(separator) `

join() 함수는 배열의 모든 값들을 연결한 문자열을 리턴합니다.

이때 각각의 값들 사이에는 파라미터로 입력된 구분자(separator)가 들어가게 됩니다.

만약, separator를 입력하지 않은 경우, default로 ','가 들어갑니다.

예시: `return squidRoute.error.errors.map(({message})=>message).join('\n');`

참조: https://hianna.tistory.com/447

# 배열 비교

- 1차원 배열 비교, 순서가 뒤죽박죽이지만 내용만 같은지를 비교할 때

```ts
accounts.length === revertedAccount.length && accounts.every((account, idx) => account === revertedAccount[idx]),

```

```ts
JSON.stringify(accounts) === JSON.stringify(revertedAccount);
```

sort메서드는 호출한 객체의 데이터를 직접 조작하기 때문에 복사한 값을 사용할 것

```ts
JSON.stringify(
                  [...accounts].sort((a, b) => {
                    if (a.id > b.id) return 1;
                    if (a.id < b.id) return -1;
                    return 0;
                  }),
                ) ===
                  JSON.stringify(
                    [...revertedAccount].sort((a, b) => {
                      if (a.id > b.id) return 1;
                      if (a.id < b.id) return -1;
                      return 0;
                    }),
                  ),
```

비교용 리스트

```ts
const testList = revertedAccount.map((item) => ({
  ...item,
  type: "ahn",
}));
```

# 중복제거

```ts
const origins = Array.from(new Set(allowedOrigins.map((item) => item.origin)));
```

# 배열 합산

```ts
const delegationAmount = useMemo(
  () =>
    delegation?.data
      ?.filter((item) => item.amount?.denom === chain.baseDenom)
      ?.reduce((ac, cu) => plus(ac, cu.amount.amount), "0") // 여기서 합산
      .toString() || "0",
  [chain.baseDenom, delegation?.data]
);
```

# 배열 합산 v2

const squidSourceChainFeePrice = useMemo(
() =>
squidRoute.data?.route.estimate.feeCosts?.reduce(
(ac, cu) =>
plus(
ac,
times(toDisplayDenomAmount(cu.amount || '0', cu.token.decimals || 0), coinGeckoPrice.data?.[cu.token.coingeckoId]?.[chromeStorage.currency] || 0),
),
'0',
) || '0',
[chromeStorage.currency, coinGeckoPrice.data, squidRoute.data?.route.estimate.feeCosts],
);

    // const squidSourceChainFeePrice = useMemo(

// () =>
// squidRoute.data?.route.estimate.feeCosts?.reduce(
// (ac, cu) =>
// plus(
// ac,
// times(toDisplayDenomAmount(cu.amount || '0', cu.token.decimals || 0), coinGeckoPrice.data?.[cu.token.coingeckoId]?.[chromeStorage.currency] || 0),
// ),
// '0',
// ) || '0',
// [chromeStorage.currency, coinGeckoPrice.data, squidRoute.data?.route.estimate.feeCosts],
// );

// const squidCrossChainFeePrice = useMemo(
// () =>
// squidRoute.data?.route.estimate.gasCosts.reduce(
// (ac, cu) =>
// plus(
// ac,
// times(toDisplayDenomAmount(cu.amount || '0', cu.token.decimals || 0), coinGeckoPrice.data?.[cu.token.coingeckoId]?.[chromeStorage.currency] || 0),
// ),
// '0',
// ) || '0',
// [chromeStorage.currency, coinGeckoPrice.data, squidRoute.data?.route.estimate.gasCosts],
// );

## 안좋은 케이스- reduce

하나의 아이디로 여러개로 분산된 객체들의 키값을 하나로 몰고 싶을때 아래와 같이 구현을 했는데
이렇게 되면 첫 순번의 객체를 제외한 나머지 객체들의 데이터가 사라지게 되어서 좋지못함

```ts
const tokenBalanceObjects = useMemo(() => {
  const copiedList = objects?.result ? [...objects.result] : [];
  return copiedList
    .filter(
      (item) =>
        getCoinType(item.data?.type) &&
        item.data?.content?.dataType === "moveObject" &&
        item.data.content.hasPublicTransfer
    )
    .map((item, _, array) => ({
      balance:
        array.reduce((ac, cu) => {
          if (
            item.data?.type === cu.data?.type &&
            cu.data?.content?.dataType === "moveObject"
          )
            // eslint-disable-next-line @typescript-eslint/no-unsafe-argument
            return plus(ac, (cu.data?.content.fields.balance as string) || "0");

          return ac;
        }, "0") || "0",
      coinType: getCoinType(item.data?.type),
      objects: [
        ...array.filter(
          (arrayItem) => arrayItem.data?.type === item.data?.type
        ),
      ],
      ...item,
      // NOTE 요 아이템 내에는 하나로 합쳐진 개개별의 오브젝트들을 모두 넣어야한다
      // NOTE 그래야 합쳐진 오브젝트들이 어떤값을 가지고 있고 그 값들의 합산으로 얼마만큼의 밸런스가 나오게 되었는지 알 수 있게되는것임.
      item: [],
    }))
    .filter(
      (object, idx, arr) =>
        arr.findIndex((item) => item.data?.type === object.data?.type) ===
          idx &&
        gt(object.balance, "0") &&
        object.coinType
    )
    .sort((coin) => (coin.coinType === SUI_COIN ? -1 : 1));
}, [objects?.result]);
```

개선 버젼

```ts
const tokenBalanceObjects = useMemo(() => {
  const copiedList = objects?.result ? [...objects.result] : [];

  const uniquebyTypeCoinList = Array.from(
    new Set([
      ...copiedList.filter(
        (item) =>
          getCoinType(item.data?.type) &&
          item.data?.content?.dataType === "moveObject" &&
          item.data.content.hasPublicTransfer
      ),
    ])
  );

  return uniquebyTypeCoinList
    .map((coin) => ({
      balance:
        copiedList
          .filter(
            (item) =>
              coin.data?.type === item.data?.type &&
              item.data?.content?.dataType === "moveObject"
          )
          .reduce((ac, cu) => {
            if (cu.data?.content?.dataType === "moveObject")
              // eslint-disable-next-line @typescript-eslint/no-unsafe-argument
              return plus(
                ac,
                (cu.data?.content.fields.balance as string) || "0"
              );

            return ac;
          }, "0") || "0",
      coinType: getCoinType(coin.data?.type),
      objects: [
        ...copiedList.filter((item) => coin.data?.type === item.data?.type),
      ],
    }))
    .sort((coin) => (coin.coinType === SUI_COIN ? -1 : 1));
}, [objects?.result]);
```

# 정렬

원하는 데이터 맨 앞으로

```tsx
suiCoinObjects.sort((a) => {
  if (a.coinType === SUI_COIN) {
    return -1;
  }
  return 1;
});
```

금단의 기술

```ts
const \_ = require('lodash');

let originalList = [
{ a: 'a', rpc: ['1', '2'] },
{ a: 'b', rpc: ['3', '4'] }
];

let newList = \_.flatMap(originalList, obj => {
return obj.rpc.map(rpc2 => {
return {
a: obj.a,
rpc2: rpc2
};
});
});

console.log(newList);

[
{ a: 'a', rpc2: '1' },
{ a: 'a', rpc2: '2' },
{ a: 'b', rpc2: '3' },
{ a: 'b', rpc2: '4' }
]
```

\_.flatMap() 함수는 리스트(배열)에서 각 요소(element)에 대해 주어진 함수를 실행한 후, 그 결과들을 하나의 리스트로 합쳐서 반환합니다. 위 예제 코드에서는 원본 리스트의 각 객체에 대해 obj.rpc 배열을 매핑(map)하고, 이를 하나의 리스트로 합쳐서 반환하도록 하였습니다.

그리고 매핑(map)된 객체는 rpc2 필드를 추가하여 새로운 객체를 생성하도록 하였습니다. 이때 return문 안에서 중괄호({}) 대신 괄호()를 사용하면, 해당 객체가 바로 반환됩니다.

## flat()

중첩배열 평탄화

```ts
const arr1 = [1, 2, [3, 4]];
arr1.flat();
// [1, 2, 3, 4]

const arr2 = [1, 2, [3, 4, [5, 6]]];
arr2.flat();
// [1, 2, 3, 4, [5, 6]]

const arr3 = [1, 2, [3, 4, [5, 6]]];
arr3.flat(2);
// [1, 2, 3, 4, 5, 6]

const arr4 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]];
arr4.flat(Infinity);
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

# forEach

단순히(return이나 map처럼 새 배열을 만들고 싶지 않을때)
배열을 순회하면서 해당 객체를 참조해 사용하고 싶을 때 사용

```ts
nftNameList.forEach((item) => {
  infos.push({ type: "all", name: item, count: nftObjects.length });
});
```
