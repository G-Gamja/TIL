# 참조 
https://docs.1inch.io/docs/aggregation-protocol/api/swagger
# available swap token list
`https://api.1inch.io/v5.0/1/tokens`

response
```json
    "0x04fa0d235c4abf4bcf4787af4cf447de572ef828": {
      "symbol": "UMA",
      "name": "UMA Voting Token v1",
      "decimals": 18,
      "address": "0x04fa0d235c4abf4bcf4787af4cf447de572ef828",
      "logoURI": "https://tokens.1inch.io/0x04fa0d235c4abf4bcf4787af4cf447de572ef828.png",
      "tags": [
        "tokens"
      ]
    },
    "0x08d967bb0134f2d07f7cfb6e246680c53927dd30": {
      "symbol": "MATH",
      "name": "MATH Token",
      "address": "0x08d967bb0134f2d07f7cfb6e246680c53927dd30",
      "decimals": 18,
      "logoURI": "https://tokens.1inch.io/0x08d967bb0134f2d07f7cfb6e246680c53927dd30.png",
      "tags": [
        "tokens"
      ]
    },
```

# allowance check

The allowance API allows the user to set a limit on the amount of assets that can be spent or traded in a single transaction without further approval.

If you have not previously exchanged this asset using 1inch Aggregation Protocol, then the following value will be displayed in the console: > Allowance: 0

This means that at the moment 1inch router is allowed to exchange 0 tokens for your wallet.

### 1inch router는 무엇인가
"1inch Router" refers to a routing engine in the 1inch Network that finds the most optimal path across different decentralized exchanges to execute a trade. It helps ensure the best available prices, lower fees, and faster execution times on advanced order types and strategies.

# quote - 교환 루트를 찾기 위해서 사용
```
curl -X 'GET' \
  'https://api.1inch.io/v5.0/1/quote?fromTokenAddress=0xdAC17F958D2ee523a2206206994597C13D831ec7&toTokenAddress=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&amount=100' \
  -H 'accept: application/json'
```
/1/quote에서 1은 chain id를 의미함
`https://api.1inch.io/v5.0/1/quote?fromTokenAddress=0xdAC17F958D2ee523a2206206994597C13D831ec7&toTokenAddress=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&amount=100`

// response

```json
{
  "fromToken": {
    "symbol": "USDT",
    "name": "Tether USD",
    "address": "0xdac17f958d2ee523a2206206994597c13d831ec7",
    "decimals": 6,
    "logoURI": "https://tokens.1inch.io/0xdac17f958d2ee523a2206206994597c13d831ec7.png",
    "tags": [
      "tokens",
      "PEG:USD"
    ]
  },
  "toToken": {
    "symbol": "USDC",
    "name": "USD Coin",
    "address": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
    "decimals": 6,
    "logoURI": "https://tokens.1inch.io/0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.png",
    "eip2612": true,
    "domainVersion": "2",
    "tags": [
      "tokens",
      "PEG:USD"
    ]
  },
  "toTokenAmount": "99",
  "fromTokenAmount": "100",
  "protocols": [
    [
      [
        {
          "name": "SUSHI",
          "part": 100,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"
        }
      ]
    ]
  ],
  "estimatedGas": 223000
}
```


- swap api params: https://docs.1inch.io/docs/aggregation-protocol/api/swap-params
- quote api params: https://docs.1inch.io/docs/aggregation-protocol/api/quote-params
    - 주의할점
    - do not use estimatedGas from the quote method as the gas limit of a transaction
    - 리턴값의 estimatedGas을 gas limit값으로 그대로 사용하지는 말 것