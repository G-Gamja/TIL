# Squid Swap SDK Docs 정리

## 설치 : `yarn add @0xsquid/sdk`

## init & config setting

```ts
import { Squid } from "@0xsquid/sdk";

(async () => {
  // instantiate the SDK
  const squid = new Squid({
    baseUrl: "https://testnet.api.0xsquid.com", // for mainnet use "https://api.0xsquid.com"
  });
  // init the SDK
  await squid.init();
  console.log("Squid inited");
})();
```

### 고급 설정

```
const Squid = new Squid(config)

type Config = {
  apiKey?: string : 추후 구현 예정
  baseUrl?: string : 메인, 테스트넷 설정
  executionSettings?: {
    infiniteApproval?: boolean : 스왑 amount 제한 걸고 싶을때 false: default가 infinite
  }
}
```

## 지원 토큰, 체인

```ts
const tokens = squid.tokens as TokenData[];
const chains = squid.chains as ChainData[];
```

토큰 필터링 로직 예시

ex 이더리움에서 지원하는 토큰

```ts
const availableEthTokens = squid.tokens.map(
  (item) => item.chainId === ETHEREUM.ChainId
);
```

```ts
// set fromToken to USDC on ethereum
const fromToken = squid.tokens.find(
  (t) =>
    // display Denom
    t.symbol === "USDC" &&
    // chain id는 익스텐션에서 사용하는 값과 동일함
    t.chainId === squid.chains.find((c) => c.chainName === "Ethereum")?.chainId
);
```

여기서 c.chainName은 axelar sdk에서 사용하는 값을 사용한다 - https://docs.axelar.dev/dev/build/chain-names/mainnet
지원하는 토큰의 종류 또한 axelar sdk와 같다  
 - https://docs.axelar.dev/dev/build/contract-addresses/mainnet

## Transfer Params

USDC와 같이 동일 이름으로 여러 체인에서 지원하는 토큰의 경우 체인 별 해당 토큰의 컨트랙 주소가 다를 수 있기에 꼭 확인할 것

```ts
const params = {
  fromChain: 5, // Goerli testnet
  fromToken: "0xB4FBF271143F4FBf7B91A5ded31805e42b2208d6", // WETH on Goerli
  fromAmount: "50000000000000000", // 0.05 WETH
  toChain: 43113, // Avalanche Fuji Testnet
  toToken: "0x57f1c63497aee0be305b8852b354cec793da43bb", // aUSDC on Avalanche Fuji Testnet
  toAddress: "0xAD3A87a43489C44f0a8A33113B2745338ae71A9D", // the recipient of the trade
  slippage: 1.0, // 1.00 = 1% max slippage across the entire route 1~99.9까지 설정 가능
  enableForecall: true, // instant execution service, defaults to true
  // Instant execution on the destination chain when set to true. Defaults to true.
  // 이게 설정되어있어야 따로 컨트랙이 토큰의 allowance를 확보하기 위한 tx를 생성하지 않아도 된다
  // allowance를 체크하고 싶다면 SDK의 allowance메서드를 사용할 것
  quoteOnly: false, // optional, defaults to false
  // If `true`, returns only a quote for the route. Omits transaction data needed for execution. Defaults to `false
};
```

## allowance

기본적으로는 params에 enableForcall을 true로 설정해주면 추가적으로 allowance tx를 생성하지 않아도 된다.

The `enableForecall` command in the Squid SDK allows you to turn on the implicit allowance for tokens for smart contracts. This means that contracts can allocate tokens for use of specific functions without explicit call to the token contract itself.

You can check token allowances by calling the `allowance` method from the token contract, passing the account address and the contract address you wish to check.

## 스왑 루트 구하기

params은 위에서 선언한 값을 넣어주면 된다.

```ts
const { route } = await squid.getRoute(params);
```

해당값의 페이로드는 routeResponse.md를 참고하면 된다.

## 스왑 실행하기

```ts
const tx = await squid.executeRoute({ signer, route, executionSettings });

const txReceipt = await tx.wait();
```

`executeRoute`의 옵션 파람으로 `executionSettings`을 넣을 수 있는데

```ts
type executionSettings?: {
    infiniteApproval?: boolean;
    setGasPrice: boolean;
};
```

`infiniteApproval` defaults to `true` if not set by the user. This will set the SquidRouter contract's `allowance` of fromToken to `max`.
When infiniteApproval is false, the Squid SDK will approve only the amount necessary for this trade.

#### allowance check

1inch와 달리 하나의 tx만으로 allowance뚫는 불편한 과정이 필요없는 것으로 보임

## Route Response필드 추가 설명

- aggregatePriceImpact
  - 모든 트레이드의 토탈 price impact를 의미함
- feeCosts
  - 루트별 각 단계에서 지불된 fee의 종류를 의미함
  - https://docs.squidrouter.com/architecture/transaction-times-and-fees
- route.estimate.route
  - 토큰들이 어떤 dex를 거치고 중간에 어떤 토큰으로 변환되는지 그 과정의 데이터를 담고 있음

## Fee

스퀴드 라우터 자체에서는 수수료를 받고 있진 않지만 거치는 각 체인에서 수수료(가스비)를 뗴가고 있음

- 일반적인 스왑에 대한 수수료
  - 대상(수신) 체인에 스왑이 있는 트랜잭션의 경우 소스(송신) 체인의 네이티브 토큰을 사용하여 수수료를 지불합니다. 이 수수료는 Axelar의 중계자에게 지급되며, 이들은 신뢰 없이 체인을 통해 메시지와 증명을 전달합니다. Axelar의 릴레이어는 모든 체인에 가스를 보유하고 거래에 대한 예상 가스 가격을 얻기 위해 Squid가 호출하는 가격 책정 API를 제공합니다. 예를 들어 대상 거래가 아마도 50센트의 AVAX 가스 비용(수신체인이 아발란체여서 AVAX를 수수료로 받음)이 든다면 Squid는 사용자에게 Axelar가 백엔드에서 제공하는 가격에 따라 Axelar의 가스 서비스에 50센트 상당의 Moonbeam(송신체인이 문빔이어서 문빔의 코인인 Moonbeam을 채택)을 지불하도록 지시할 것입니다.
- 브릿지 비용
  - axelar sdk에서 보았던 그 수수료 정책과 동일함
  - https://docs.axelar.dev/resources/mainnet#cross-chain-relayer-gas-fee

https://docs.widget.squidrouter.com/gas-fees-and-refunds

# 수신 체인에서 가스 price가 스파이크를 치는 경우

수신체인에서 갑자기 수수료가 스파이크를 치는 경우 tx가 stuck될 수 있는데
이럴때 두가지 옵션이 있다. - 가스비가 줄어들기를 기다리거나 - 사용자가 직접 가스비를 추가 지불할 수 있다

가스비를 따로 추가 지불하기 위해선 Axelar Scan에 직접 tx를 검색해서 `Add Gas`를 통해 추가 가스비를 지불할 수 있다.

- 스퀴드 SDK는 자체적으로 가스비를 추가 지불할 수 있도록 코드를 구현중에 있음(추가 가스 지불 기능 미구현)

# TX 처리 소요시간

보통은 이렇다

- To Ethereum 30sec - 2min
- From Ethereum - 15-25min

## Settings

https://docs.widget.squidrouter.com/settings#infinite-approval

### swagger

https://squidrouter.readme.io/reference/get_route

## Express 설정

크로스 체인 실행의 모드를 2개로 설정할 수 있는데 파라미터에 expressEnabled를 추가하면 됨

- 실행속도가 비약적으로 빨라짐과 동시에 추가 수수료가 발생함

https://docs.squidrouter.com/aggregators#express-and-regular-transactions
