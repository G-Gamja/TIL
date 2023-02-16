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

### 추천인 
- swap api중 param에 referrer의 주소가 들어가는데 아마 이곳에 주소를 넣으면 우리한테 수수료의 일부를 넘겨주는 방식인 것 같음
- fee param에서 수수료의 몇퍼센트를 추천인이 가져갈 지를 정할 수 있음
  - this percentage of fromTokenAddress token amount will be sent to referrerAddress, the rest will be used as input for a swap
    min: 0; max: 3; default: 0  example to set a fee to 1.5%: &fee=1.5

- swap api params: https://docs.1inch.io/docs/aggregation-protocol/api/swap-params
- quote api params: https://docs.1inch.io/docs/aggregation-protocol/api/quote-params
    - 주의할점
    - do not use estimatedGas from the quote method as the gas limit of a transaction
    - 리턴값의 estimatedGas을 gas limit값으로 그대로 사용하지는 말 것
- 여기서 abi는 1inch의 스마트 컨트랙을 호출하여 어떤 값을 가져올 때 사용될 건데 우리는 swap api를 통해 tx에 넣을 콜 데이터를 바로 얻을 수 있기 때문에 굳이 필요하지 않을 듯 싶다

샘플 코드

```ts
import * as ethers from 'ethers';

const provider = new ethers.providers.InfuraProvider('homestead', 'Your_Infura_API_Key');
const contractAddress = '0x1111111111111111111111111111111111111111';
const abi = [{...}];  // ABI for the 1inch smart contract
    
async function swap(fromToken: string, toToken: string, amount: string) {
    const [quote] = await fetch(`https://api.1inch.exchange/v1.1/quote?
        from=${fromToken}&to=${toToken}&amount=${amount}`).then(res => res.json());
        
    const oneInchContract = new ethers.Contract(contractAddress, abi, provider);
    const swapData = oneInchContract.interface.functions.swap.encode([
        quote.results[0].path,
        quote.results[0].to,
        quote.results[0].value
    ]);
    
    const wallet = new ethers.Wallet('Your_Private_Key', provider);
    const tx = await wallet.sendTransaction({
        to: contractAddress,
        value: 0,
        data: swapData
    });
    
    console.log(`Transaction hash: ${tx.hash}`);
    return tx;

}
```
# 프로토콜 프로퍼티 내 part의 의미
In the response of the 1inch quote api, the "part" field means that an amount of a particular token must be split between multiple protocols as part of an exchange. An example of when the "part" field is not equal to 100 would be if you are exchanging two tokens and one of them is divided among multiple liquidity protocols. For example, let's say you need to exchange 10 ETH for DAI. The response could look like this:

```json
{
    "fromToken": {
        "symbol": "ETH",         
        "name": "Ethereum",
        "decimals": 18,
        "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
        "logoURI": "https://tokens.1inch.io/0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee.png",
        "tags": [
            "native",
            "PEG:ETH"
        ]
    },
    "toToken": {
        "symbol": "DAI",
        "name": "Dai Stablecoin v1.0",
        "decimals": 18,
        "address": "0x111111111117dc0aa78b770fa6a738034120c302",
        "logoURI": "https://tokens.1inch.io/0x111111111117dc0aa78b770fa6a738034120c302.png",
        "eip2612": true,
        "tags": [
            "token"
        ]
    },
    "toTokenAmount": "8999233919311950472",
    "fromTokenAmount": "100000000000000000",
    "protocols": [
        [
            [
                {
                    "name": "UNISWAP_V2",
                    "part": 50, 
                    "fromTokenAddress": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
                    "toTokenAddress": "0x111111111117dc0aa78b770fa6a738034120c302"
                },
                {
                    "name": "KYBER",
                    "part": 50, 
                    "fromTokenAddress": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee"
                    "toTokenAddress": "0x111111111117dc0aa78b770fa6a738034120c302"
                }
            ]
        ]
    ],
    "estimatedGas": 152065
}
```
In this example, 10 ETH is split into 2 payments of 5 ETH each, where 5 gets exchanged on Uniswap and 5 gets exchanged on Kyber. The "part" value indicates the size of the payment (50% for Uniswap and 50% for Kyber).

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
    "symbol": "RPL",
    "name": "Rocket Pool Protocol",
    "decimals": 18,
    "address": "0xd33526068d116ce69f19a9ee46f0bd304f21a51f",
    "logoURI": "https://tokens.1inch.io/0xd33526068d116ce69f19a9ee46f0bd304f21a51f.png",
    "tags": [
      "tokens"
    ]
  },
  "toTokenAmount": "202460029385482257274234",
  "fromTokenAmount": "10000000000000000",
  "protocols": [
    [
      [
        {
          "name": "ONE_INCH_LP_1_1",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "DEFISWAP",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "CURVE",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "SADDLE",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "BALANCER_V2",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "STABLE_PLAZA",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "SYNAPSE",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "XSIGMA",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "MOONISWAP",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "CURVE",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "UNISWAP_V3",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "S_FINANCE",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "NOMISWAP_STABLE",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "SADDLE",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "DEFI_PLAZA",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "CURVE",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "SWERVE",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "DODO_V2",
          "part": 2,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "VERSE",
          "part": 4,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "CURVE",
          "part": 4,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "CURVE",
          "part": 18,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        },
        {
          "name": "UNISWAP_V2",
          "part": 38,
          "fromTokenAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "toTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f"
        }
      ],
      [
        {
          "name": "VERSE",
          "part": 2,
          "fromTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f",
          "toTokenAddress": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee"
        },
        {
          "name": "DEFISWAP",
          "part": 2,
          "fromTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f",
          "toTokenAddress": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee"
        },
        {
          "name": "UNISWAP_V1",
          "part": 2,
          "fromTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f",
          "toTokenAddress": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee"
        },
        {
          "name": "SHIBASWAP",
          "part": 2,
          "fromTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f",
          "toTokenAddress": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee"
        },
        {
          "name": "DXSWAP",
          "part": 2,
          "fromTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f",
          "toTokenAddress": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee"
        },
        {
          "name": "MOONISWAP",
          "part": 2,
          "fromTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f",
          "toTokenAddress": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee"
        },
        {
          "name": "UNISWAP_V3",
          "part": 10,
          "fromTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f",
          "toTokenAddress": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee"
        },
        {
          "name": "SUSHI",
          "part": 26,
          "fromTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f",
          "toTokenAddress": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee"
        },
        {
          "name": "UNISWAP_V2",
          "part": 52,
          "fromTokenAddress": "0x6b175474e89094c44da98b954eedeac495271d0f",
          "toTokenAddress": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee"
        }
      ],
      [
        {
          "name": "UNISWAP_V3",
          "part": 2,
          "fromTokenAddress": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
          "toTokenAddress": "0xd33526068d116ce69f19a9ee46f0bd304f21a51f"
        },
        {
          "name": "UNISWAP_V3",
          "part": 98,
          "fromTokenAddress": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
          "toTokenAddress": "0xd33526068d116ce69f19a9ee46f0bd304f21a51f"
        }
      ]
    ]
  ],
  "estimatedGas": 7590956
}