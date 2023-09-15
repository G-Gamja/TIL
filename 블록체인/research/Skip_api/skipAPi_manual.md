# skip api manual

[skip-api-docs](https://api-docs.skip.money/reference)
[skip-api-docs-swagger](https://api-swagger.skip.money/#/Fungible/getRoute)

# swap 루트 쿼리

```ts
const requestURL = "https://api.skip.money/v1/fungible/route";

const fetcher = async ({ fetchUrl, skipRouteParam }: FetchProps) =>
  post<SkipRoutePayload>(fetchUrl, {
    // 스왑 하고 싶은 만큼의 토큰 수량입니다. decimal이 곱해지지 않은 값입니다.
    amount_in: skipRouteParam?.amountIn,
    // 스왑 하고 싶은 토큰의 denom입니다.(ex. uosmo)
    source_asset_denom: skipRouteParam?.sourceAssetDenom,
    // 송신 체인의 체인 id 값입니다.(ex. osmosis-1)
    source_asset_chain_id: skipRouteParam?.sourceAssetChainId,
    // 스왑되어 받고 싶은 토큰의 denom입니다.(ex. uatom)
    dest_asset_denom: skipRouteParam?.destAssetDenom,
    // 수신 체인의 체인 id 값입니다.(ex. cosmoshub-4)
    dest_asset_chain_id: skipRouteParam?.destAssetChainId,
    // 스왑에 대한 수수료로 받을 토큰의 비율입니다. (ex. '100')
    // 100이면 1%의 수수료를 받습니다.
    cumulative_affiliate_fee_bps: skipRouteParam?.cumulativeAffiliateFeeBps,
  });


// 예시 파라미터

{
  "amount_in": "1000000",
  "source_asset_denom": "uusdc",
  "source_asset_chain_id": "axelar-dojo-1",
  "dest_asset_denom": "uatom",
  "dest_asset_chain_id": "cosmoshub-4",
  "cumulative_affiliate_fee_bps": "0",
  // 필수값은 아닙니다.
  "client_id": "skip-api-docs"
}

```

client_id가 추가되었습니다. 다음과 같은 목적으로 사용됩니다.

- Poking you when you're using a feature that's about to be deprecated or upgraded
- Proactively reaching out if you or your users trip a pesky bug
- Understanding which features are actually valuable to you and which aren't without you having to tell us in DMs or on a call

https://api-docs.skip.money/changelog/august-30th-2023-optimizing-speed

# swap message

```ts
const requestURL = "https://api.skip.money/v1/fungible/msgs";

const fetcher = async ({ fetchUrl, skipSwapTxParam }: FetchProps) =>
  post<SkipSwapTxPayload>(fetchUrl, {
    source_asset_denom: skipSwapTxParam?.source_asset_denom,
    source_asset_chain_id: skipSwapTxParam?.source_asset_chain_id,
    dest_asset_denom: skipSwapTxParam?.dest_asset_denom,
    dest_asset_chain_id: skipSwapTxParam?.dest_asset_chain_id,
    amount_in: skipSwapTxParam?.amount_in,
    address_list: skipSwapTxParam?.addresses,
    operations: skipSwapTxParam?.operations,
    amount_out: skipSwapTxParam?.amount_out,
    slippage_tolerance_percent: skipSwapTxParam?.slippage,
    affiliates: skipSwapTxParam?.affiliates || [],
  });
```

예시 파라미터입니다.

```json
{
  "source_asset_denom": "uusdc",
  "source_asset_chain_id": "axelar-dojo-1",
  "dest_asset_denom": "uatom",
  "dest_asset_chain_id": "cosmoshub-4",
  "amount_in": "1000000",
  //
  "amount_out": "107033",
  // swap route 쿼리로 나온 data의 chain_ids에 포함된 체인의 주소를 사용합니다.
  "address_list": [
    "axelar1x8ad0zyw52mvndh7hlnafrg0gt284ga7u3rez0",
    "osmo1x8ad0zyw52mvndh7hlnafrg0gt284ga7syxplu",
    "cosmos1x8ad0zyw52mvndh7hlnafrg0gt284ga7cl43fw"
  ],

  // swap route 쿼리로 나온 값의 operation
  "operations": [
    {
      "transfer": {
        "port": "transfer",
        "channel": "channel-3",
        "chain_id": "axelar-dojo-1",
        "pfm_enabled": false,
        "dest_denom": "ibc/D189335C6E4A68B513C10AB227BF1C1D38C746766278BA3EEB4FB14124F1D858"
      }
    },
    {
      "swap": {
        "swap_in": {
          "swap_venue": {
            "name": "osmosis-poolmanager",
            "chain_id": "osmosis-1"
          },
          "swap_operations": [
            {
              "pool": "678",
              "denom_in": "ibc/D189335C6E4A68B513C10AB227BF1C1D38C746766278BA3EEB4FB14124F1D858",
              "denom_out": "uosmo"
            },
            {
              "pool": "1",
              "denom_in": "uosmo",
              "denom_out": "ibc/27394FB092D2ECCD56123C74F36E4C1F926001CEADA9CA97EA622B25F41E5EB2"
            }
          ],
          "swap_amount_in": "1000000"
        },
        "estimated_affiliate_fee": "0ibc/27394FB092D2ECCD56123C74F36E4C1F926001CEADA9CA97EA622B25F41E5EB2"
      }
    },
    {
      "transfer": {
        "port": "transfer",
        "channel": "channel-0",
        "chain_id": "osmosis-1",
        "pfm_enabled": true,
        "dest_denom": "uatom"
      }
    }
  ],
  "slippage_tolerance_percent": "1",
  "affiliates": [],
  "client_id": "skip-api-docs"
}
```
