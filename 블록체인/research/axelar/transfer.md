- axelar가 특정 체인의 입금 주소를 생성하고 이를 모니터링하는 악셀러가
-입금 주소는 요청자를 대신하여 `Axelar의 Relayer Services`에서 생성하고 모니터링하는 특수 주소입니다. 이는 중앙 집중식 거래소가 토큰 전송을 용이하게 하는 모니터링되는 일회성 예금 주소를 생성하는 방법과 유사합니다.

- 네이티브 토큰을 전송시에는 토큰이 `wrap`된 토큰이 전송됩니다

- `wrap`된 토큰을 원래의 체인으로 send하는 것을 `unwrap`이라고 한다

# axelar transfer process (간략하게)
-   The user generates a deposit address on a specific source chain./ 유저가 특정 체인에 입금 주소를 생성함(sdk(`AxelarQueryAPI`),cli로 생성가능)
-   The user sends tokens to the deposit address on the source chain./ 유저가 그 입금 주소로 send함
    -   IMPORTANT NOTE: When making your deposit, please ensure that the amount is greater than the cross-chain relayer gas fee
    - send시에 릴레이어에서 정한 가스 수수료보다 높은 값을 송금해야함, 그렇지 않으면 입금 주소로 들어온 토큰의 양이 수수료 보다 많아질때까지 프로세스가 멈출 것임
    -  이 계산 수수료를 초과하지 않는 입금 주소로의 입금은 해당 입금 주소가 수수료를 포함하도록 충전될 때까지 처리되지 않습니다.    
    -   A table of fees is listed here: mainnet: https://docs.axelar.dev/resources/mainnet#cross-chain-relayer-gas-fee
-   Axelar relayers observe the deposit transaction on the source chain and complete it on the destination chain,` assuming the amount exceeds the requisite fee.`
-   Watch your tokens arrive on the destination chain.


# 가스 수수료 estimate값 얻기

송금시의 토큰양이 릴레이어의 수수료 요구보다 적으면 안되니까 아래의 코드로 예상되는 수수료 수치를 얻을 수 있음

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
```ts
interface GetDepositAddressParams {
  fromChain: string; //  Chain IDs (as recognized by Axelar) must be used here. For example, in testnet, the chain ID for Osmosis is osmosis-5.
  toChain: string; //  Chain IDs (as recognized by Axelar) must be used here. For example, in testnet, the chain ID for Osmosis is osmosis-5.
  destinationAddress: string; //  ex) axelar1aygdt8742gamxv8ca99wzh56ry4xw5s3p7dnu6
  asset: string; // denom of asset. See note (2) below // For all the assets that Axelar supports natively, the network identifies the asset by a denom. If you are accustomed to the symbol typically used on EVM chains, you will have to convert that symbol to a denom. The SDK has an API method you can use to convert symbol to denom: getDenomFromSymbol
  options?: {
    shouldUnwrapIntoNative?: boolean;
    refundAddress?: string; //=  Refund address is an optional parameter. It specifies the address on a source chain where tokens erroneously deposited into a deposit address can be refunded. For example, if a deposit address was generated to send USDC, but the user mistakenly deposits WAVAX. If no address is specified, the API defaults the parameter to the Gas Receiver contract that can refund on the user's behalf. At the moment, the refundAddress parameter is only compatible for use cases 3 and 4 above, wrap and unwrap cases, respectively.
  };
}

async getDepositAddress({
  fromChain, toChain, destinationAddress, asset
}: GetDepositAddressParams): Promise<string> {}
```
## transfer case
1. 코스모스 생태계 내에서 transfer: 단순히 IBC Transfer하면 되고 수신인 주소만 `생성된 입금주소`로 넣으면 됨
2. 오스모 -> 이더리움
    - 똑같이 IBC Send 로 넘어감

해당 tx메시지 바디
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
## unwrap해서 보내는 경우
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
    // 해당 변수를 통해 wrap된 토큰을 그냥 보낼건지 unwrap해서 보낼지를 결정할 수 있음
    shouldUnwrapIntoNative: false // -> Wavax(wrapped): 폴리곤 -> 아발란체
    shouldUnwrapIntoNative: true // -> Avax(Unwrapped): 폴리곤 -> 아발란체
    
  }
});
```
# transaction check
트랜잭션이 막힐 수도 있는데 그 상태값은 아래의 스캐너 디앱을 통해 확인이 가능함
https://docs.axelar.dev/dev/monitor-recover/recovery
# 주의사항

- 오스모 -> 이더리움: 악셀러 코인
The recipient will receive wAXL on Ethereum. If your recipient doesn’t support wAXL the funds will be lost!
All ERC20 token addresses can be verified here.
# reference
axelar transfer api docs: https://docs.axelar.dev/dev/axelarjs-sdk/token-transfer-dep-addr