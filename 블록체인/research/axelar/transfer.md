# Axelar Cross-Chain Transfer
악셀러를 이용해서 체인간에 토큰을 보내는 방식은 아래 2가지와 같습니다.

- Call `sendToken` on an Axelar gateway EVM contract.
- Get a deposit address using the AxelarJS SDK (우리가 사용할 방법).


- Axelar가 특정 체인의 입금 주소를 생성하고 이를 모니터링하는 `Axelar's Relayer Services`가 해당 주소로 입금되면 이를 수신을 원하는 체인으로 넘겨주는 방식입니다.
  - 입금 주소는 요청자를 대신하여 `Axelar의 Relayer Services`에서 생성하고 모니터링하는 특수 주소입니다. 이는 중앙 집중식 거래소가 토큰 전송을 용이하게 하는 모니터링되는 일회성 예금 주소를 생성하는 방법과 유사합니다.
    - 원문: A deposit address is a special address created and monitored by Axelar's Relayer Services on behalf of the requester. It is similar to how centralized exchanges generate a monitored, one-time deposit address that facilitates your token transfers.

- 네이티브 토큰을 전송시에는 `wrap`된 토큰이 전송됩니다

- `wrap`된 토큰을 원래의 체인으로 send하는 것을 `unwrap`이라고 하며, 임시주소 생성시 옵션필드를 활용해서 `unwrap`여부를 설정할 수 있습니다.

# Axelar transfer process (간략하게)
-   The user generates a deposit address on a specific source chain./ 유저가 특정 체인의 입금 주소를 생성함(sdk(`AxelarQueryAPI`) 혹은 cli로 생성가능)
-   The user sends tokens to the deposit address on the source chain./ 유저가 그 입금 주소로 토큰을 send함
    - send시에 릴레이어에서 정한 가스 수수료보다 높은 값을 송금해야함, 그렇지 않으면 입금 주소로 들어온 토큰의 양이 수수료 보다 많아질때까지 프로세스가 중단됨.
    -  원문: When making your deposit, please ensure that the amount is greater than the cross-chain relayer gas fee
    -  메인 넷의 가스 수수료 테이블: https://docs.axelar.dev/resources/mainnet#cross-chain-relayer-gas-fee
-   Axelar relayers observe the deposit transaction on the source chain and complete it on the destination chain,` assuming the amount exceeds the requisite fee.`
-   Watch your tokens arrive on the destination chain.


# 가스 수수료 estimate값 얻기
- 정해진 수수료(relat) 미만의 값을 생성된 주소로 transfer한 경우 정해진 수수료 이상의 값이 들어올 때 까지 프로세스가 중지됩니다
  - 수수료는 어떤 체인을 거치느냐에 따라 하드코딩된 값을 사용하고 있음(ex. 오스모시스에서 이더리움으로 USDC를 보낼 때  오스모시스(0.1 USDC) + 이더리움(10.4 USDC) = 10.5 USDC)
- 원문: If you plan on using the transfer assets functionality, it is paramount to take the relayer fees into account. Any deposits into a deposit address that are not in excess of this calculate fee will not get processed until that deposit address is topped up to encompass the fee.

송금시의 토큰양이 릴레이어가 요구하는 수수료보다 적으면 안되니까 아래의 api로 예상되는 수수료 값을 얻을 수 있습니다.

```ts
import {
  AxelarQueryAPI,
  CHAINS,
  Environment,
} from "@axelar-network/axelarjs-sdk";

async function main() {
  const axelarQuery = new AxelarQueryAPI({
    environment: Environment.TESTNET,
  });

  const fee = await axelarQuery.getTransferFee(
    CHAINS.TESTNET.OSMOSIS,
    CHAINS.TESTNET.AVALANCHE,
    "uausdc",
    1000000
  );
  // returns  { fee: { denom: 'uausdc', amount: '150000' } }
}

main();
```
# 입금 주소 생성하기
argument로 넘길 Chain ID, denom의 값은 api에서 제공하는 상수를 사용하는 것을 추천드립니다.
```ts
interface GetDepositAddressParams {
  fromChain: string; //  Chain IDs (as recognized by Axelar) must be used here. For example, in testnet, the chain ID for Osmosis is osmosis-5.
  toChain: string; //  Chain IDs (as recognized by Axelar) must be used here. For example, in testnet, the chain ID for Osmosis is osmosis-5.
  destinationAddress: string; //  ex) axelar1aygdt8742gamxv8ca99wzh56ry4xw5s3p7dnu6
  asset: string;
   // denom of asset. For all the assets that Axelar supports natively, 
  // the network identifies the asset by a denom. If you are accustomed to the symbol typically used on EVM chains, you will have to convert that symbol to a denom.
  // The SDK has an API method you can use to convert symbol to denom: getDenomFromSymbol
  options?: {
    shouldUnwrapIntoNative?: boolean;
    refundAddress?: string; 
    // Refund address is an optional parameter. It specifies the  
    // address on a source chain where tokens erroneously deposited into a deposit address
    // can be refunded. For example, if a deposit address was generated to send USDC,
    // but the user mistakenly deposits WAVAX. If no address is specified, 
    // the API defaults the parameter to the Gas Receiver contract that can refund on the user's behalf. At the moment, the refundAddress parameter is only compatible for use cases 3 and 4 above,
    // wrap and unwrap cases, respectively.
  };
}

async getDepositAddress({
  fromChain, toChain, destinationAddress, asset
}: GetDepositAddressParams): Promise<string> {}
```
예시
- 상수로 제공되는 chain id는 main net과 test net의 값이 상이합니다.
```ts
const sdk = new AxelarAssetTransfer({ environment: "testnet" });

const fromChain = CHAINS.TESTNET.OSMOSIS, 
  toChain = CHAINS.TESTNET.AVALANCHE,
  destinationAddress = "0xF16DfB26e1FEc993E085092563ECFAEaDa7eD7fD",
  asset: "uausdc"; 

const depositAddress = await sdk.getDepositAddress({
  fromChain, 
  toChain, 
  destinationAddress, 
  asset
});
```

## 토큰을 unwrap해서 보내는 경우
```ts
const sdk = new AxelarAssetTransfer({ environment: "testnet" });

const fromChain = CHAINS.TESTNET.POLYGON, 
  toChain = CHAINS.TESTNET.AVALANCHE,
  destinationAddress = "0xF16DfB26e1FEc993E085092563ECFAEaDa7eD7fD",
  asset: "wavax-wei"

const depositAddress = await sdk.getDepositAddress({
  fromChain, 
  toChain, 
  destinationAddress, 
  asset,
  options: {
    // 해당 필드를 통해 wrap된 토큰을 그냥 보낼건지 unwrap해서 보낼지를 결정할 수 있음
    shouldUnwrapIntoNative: false // -> Wavax(wrapped): 폴리곤 -> 아발란체
    shouldUnwrapIntoNative: true // -> Avax(Unwrapped): 폴리곤 -> 아발란체
    
  }
});
```

## Transfer case

### 송신 체인이 코스모스 계열인 경우
- 코스모스 계열의 체인에서(송신) transfer: IBC Transfer,Send로 토큰을 전송하면 됩니다.   수신인 주소만 `생성된 입금주소`로 넣으면 됩니다.

1. 코스모스 계열 체인에서 transfer
  - 예시 Tx
```json
{
  "txBody": {
    "messages": [
      {
        "sourcePort": "transfer",
        "sourceChannel": "channel-208",
        "token": {
          "denom": "ibc/903A61A498756EA560B85A85132D3AEE21B5DEDD41213725D22ABF276EA6945E",
          "amount": "33601480"
        },
        "sender": "osmo1aygdt8742gamxv8ca99wzh56ry4xw5s3dtgtpf",
        "receiver": "axelar1qqfncqvj68wxgu8ftdxx9zp85ruax57dju80hwr7e8hjpklur6as7my32d",
        "timeoutHeight": {
          "revisionNumber": "10",
          "revisionHeight": "10"
        },
        "timeoutTimestamp": "0"
      }
    ],
    "memo": "",
    "timeoutHeight": "0",
    "extensionOptions": [],
    "nonCriticalExtensionOptions": []
  },
  "authInfo": {
    "signerInfos": [
      {
        "publicKey": {
          "typeUrl": "/cosmos.crypto.secp256k1.PubKey",
          "value": "CiECoqz7XBbK22JBjnUSeHnKSU6RmzmcWom5Psoizic2jhY="
        },
        "modeInfo": {
          "single": {
            "mode": "SIGN_MODE_DIRECT"
          }
        },
        "sequence": "79"
      }
    ],
    "fee": {
      "amount": [
        {
          "denom": "uosmo",
          "amount": "6250"
        }
      ],
      "gasLimit": "250000",
      "payer": "",
      "granter": ""
    }
  },
  "chainId": "osmosis-1",
  "accountNumber": "706372"
}
```
 - https://axelarscan.io/transfer/5EFAE4BA8365088688FC0A065ACB9B9DE926C87A6E5D345AA1EDA5C5317BF9A8
### 이더리움 계열의 체인에서 transfer
- 이더리움 계열의 체인에서(송신) transfer: `생성된 주소(to)`로 send tx를 생성하면 됩니다.

  - https://axelarscan.io/transfer/0x59da5d709cb54792a84a7acb4b2dadce37c38d1b608e70e6efcaed3e0443dffb
# Transaction check
트랜잭션이 막힐 수도(stuck) 있는데 해결 방식은 다음과 같습니다.
  - Axelarscan UI에서 직접 해결하기
  - AxelarJS SDK에서 제공하는 api를 통해 메서드를 구현하여 해결하기
https://docs.axelar.dev/dev/monitor-recover/recovery

# Supported Chain & Token List
 Axelar Transfer를 지원하는 체인과 토큰의 종류는 첨부된 파일에 정리되어 있습니다.
  - 현재 Satellite에서 지원하지 않는 체인과 토큰의 정보도 포함되어 있음
    - injective
# 주의사항
- Maximum Transfer Amount가 존재하여 이 값보다 더 많은 양을 transfer시 transfer 프로세스가 완료되기 까지 오랜시간이 걸릴 수도 있다고 경고하고 있습니다.

- 수신 체인에서 수신할 토큰을 지원하지 않는 경우 해당 토큰은 lost됩니다. 어떤 체인과 토큰이 지원되는지는 Supported Chain & Token List에서 확인 가능합니다.
  - 원문: The recipient will receive wAXL on Ethereum. If your recipient doesn’t support wAXL the funds will be lost!

# 테스트 결과
1. 오스모시스 -> 이더리움: axlUSDC 14개 전송
  - 송신 체인이 코스모스 계열이라 axelar prefix가 들어간 주소가 생성되었고 이 주소로 IBC Send함
  - 생성된 주소: axelar1vhrcx4gxrs7f34cl74qpgn0qy0zzzq5lg5554k9sdkaslq58jrnsewfvkv
- 결과
  - 수수료 10.5개를 제외한 3.5개의 axlUSDC가 입금되었음  
  - tx
  ```json
  {
    "account_number": "706372",
    "chain_id": "osmosis-1",
    "fee": {
        "amount": [
            {
                "denom": "uosmo",
                "amount": "30000"
            }
        ],
        "gas": "250000"
    },
    "memo": "",
    "msgs": [
        {
            "type": "cosmos-sdk/MsgTransfer",
            "value": {
                "receiver": "axelar1vhrcx4gxrs7f34cl74qpgn0qy0zzzq5lg5554k9sdkaslq58jrnsewfvkv",
                "sender": "osmo1aygdt8742gamxv8ca99wzh56ry4xw5s3dtgtpf",
                "source_channel": "channel-208",
                "source_port": "transfer",
                "timeout_height": {
                    "revision_height": "10",
                    "revision_number": "10"
                },
                "token": {
                    "amount": "14000000",
                    "denom": "ibc/D189335C6E4A68B513C10AB227BF1C1D38C746766278BA3EEB4FB14124F1D858"
                }
            }
        }
    ],
    "sequence": "83"
  }

- tx hash: 5EFAE4BA8365088688FC0A065ACB9B9DE926C87A6E5D345AA1EDA5C5317BF9A8
- 악셀러스캔에서 검색 시 더 자세한 결과를 확인 가능함: https://axelarscan.io/transfer/5EFAE4BA8365088688FC0A065ACB9B9DE926C87A6E5D345AA1EDA5C5317BF9A8


# reference
axelar transfer api docs: https://docs.axelar.dev/dev/axelarjs-sdk/token-transfer-dep-addr  
swagger: https://lcd-axelar.imperator.co/static/swagger/#/  
Transfer tokens cross-chain: https://github.com/axelarnetwork/axelar-docs/blob/c57edd7dab892e301e0fea27e6156a74ae83aca9/pages/dev/build/tokens.md