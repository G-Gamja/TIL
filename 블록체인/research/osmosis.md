# Osmosis swap

# Osmosis GAMM
The GAMM module (Generalized Automated Market Maker) provides the logic to create and interact with liquidity pools on the Osmosis DEX(decentralized exchange).

중앙집중된 중간거래자 없이 스왑을 할 수 있게 해주는 프로토콜.

스왑은 유동성 풀의 에셋안에서 진행됨

누구나 유동성풀을 만들 수 있고 이 풀에 에셋 집어넣는 사람이 liquidity providers (LPs).

amm이란 AMM은 자율 거래 메커니즘을 갖춘 탈중앙화 거래소에서 사용하는 기본 프로토콜입니다. 이를 통해 거래소 및 기타 금융 기관과 같은 중앙 집중식 기관이 필요하지 않습니다. 간단히 말해서, 교환을 촉진하는 중개자 없이 두 명의 사용자가 자산을 거래할 수 있습니다.

what is dex?: https://www.binance.com/en/blog/fiat/5-popular-dex-crypto-coins-to-look-out-for-in-2022-421499824684903921

reference: https://docs.osmosis.zone/osmosis-core/modules/gamm/#transactions

txproto레포지토리: https://github.com/osmosis-labs/osmosis/blob/v7.1.0/proto/osmosis/gamm/v1beta1/tx.proto

예: https://lightrun.com/answers/cosmos-cosmjs -how-can-i-send-an-osmosis-swap-transaction
https://github.com/cosmos/cosmjs/issues/917


관련 패키지: https://github.com/osmosis-labs/osmojs/tree/8340dfc312079893141732b9b2faea8dbd6130b1
관련 패키지2: https://github.com/cosmos/cosmjs/blob/main/packages/stargate/CUSTOM_PROTOBUF_CODECS.md

https://www.npmjs.com/package/@cosmjs/stargate

## Swap Tx Structure

* 아주 도움 됨 꼭 읽어봐야 함
참조: https://docs.osmosis.zone/osmosis-core/guides/structure/arb

gamm모듈: https://github.com/osmosis-labs/osmosis/blob/13531e5bcdbd262527c916d462974b0ef01ef7a9/x/gamm/types/tx.pb.go#L282-L287

- Swap message type을 비롯해서 오스모시스 여러 메시지 타입

** 이거를 proto파일로 가져오는게 맞는거같음
https://github.com/osmosis-labs/osmojs/blob/f3aa2c880d338ef2c2d7e0878619b89f5706a37b/packages/osmojs/src/codegen/osmosis/gamm/v1beta1/tx.ts

* @cosmology/core
* 풀 id를 비롯해서 값 가져오는 패키지
https://github.com/cosmology-tech/cosmology/tree/main/packages/core#swapexactamountin
    * 슬리피지 계산 로직
``` ts

    export const calculateAmountWithSlippage = (amount, slippage) => {
  const a = new Dec(amount);
  const b = new Dec(100).sub(new Dec(slippage)).quo(new Dec(100));
  // Number(amount) * ((100 - slippage) / 100);
  return a.mul(b).toString();
};
```
해당 파일: https://github.com/cosmology-tech/cosmology/blob/2f80fdea0b3e846de6fd62f3d3c20da63c36e5bc/packages/core/src/utils/osmo/utils.ts

### slippage
슬리피지란 The difference in a coin's price between the start and end of a transaction.
인데 이게 코인 뺼때 해당 풀에서 그 코인의 예로 80프로를 빼가면 해당 코인의 가격이 급격히 바뀌겠죠? 그러면서 트레이더의 예상과 다르게 트레이드의 코스트가 늘거나 줄게 되는거임

-> so Arbitrage를 한다 왜? 유동성 풀 간의 차익 거래는 자산 가격을 중앙 집중식 플랫폼에서 보는 것과 상대적으로 일치하도록 유지하기 위해서이다.

참조 오스모시스 랩 pricing 그리고 AMM에 대한 설명: https://osmosis.gitbook.io/o/basic-concepts/amm/pricing

https://github.com/osmosis-labs/osmojs/blob/f3aa2c880d338ef2c2d7e0878619b89f5706a37b/packages/osmojs/types/codegen/osmosis/gamm/v1beta1/tx.d.ts

* liquidity pool이란
    Liquidity pools are clusters of tokens with pre-determined weights. A token's weight is how much its value accounts for the total value within the pool.
# Osmosis Pricing: 오스모시스의 가격은 어떻게 결정될까?

https://osmosis.gitbook.io/o/basic-concepts/amm/pricing

누가 한쪽의 에셋을 싹쓸이해서 반대쪽 에셋의 가격이 폭락하는 상황을 방지하기 위해
차익 거래가 이득을 못볼 상황(두 에셋의 가격이 동일,유사해질 때)까지 차익 거래 봇이 가격 떨어진 에셋을 싹쓸이한다.   
arbitrage bots swoop in to buy the underpriced Asset B for selling in other markets. Eventually, this arbitrage is no longer profitable

# Token Weight

우선 weight는 검증인의 전체 스테이킹 어마운트를 의미함  
The measure of a validator's total stake. Validators with higher weights get selected more often to propose blocks. A validator's weight is also a measure of their voting power in governance.

여기서는 스케일의 의미를 가짐

/** Estimate min amount in given a pool with asset weights or reserves with scaling factors. (AKA weighted, or stable.) */

# Price impact

변환 후의 코인의 가격에 가해질 영향

오스모 -> 코스모 일때
오스모 변환전 가격이 7573불이고 아톰이 5965불이면 26%나 아톰 가격이 박살(임팩트를)을 주는 거임

Price impact is the influence of user's individual trade over the market price of an underlying asset pair. It is directly correlated with the amount of liquidity in the pool /    Automated Market Maker (AMM). Price impact can be especially high for illiquid markets/pairs, and may cause a trader to lose a significant portion of their funds. 

https://help.1inch.io/en/articles/4585109-what-is-price-impact-vs-price-slippage-in-defi
# Swap rate - osmosis-front
```typescript
// quo는 나누기 연산자를 의미함

// WeightedPoolMath from @osmosislab-math
// https://github.com/osmosis-labs/osmosis-frontend/tree/master/packages/math#readme
// https://github.com/osmosis-labs/osmosis-frontend/blob/master/packages/math/src/pool/weighted.ts


// Dec 클래스 from @keplr-waller/unit
// npm i @keplr-wallet/unit
const oneDec = new Dec(1);

export function calcSpotPrice(
  tokenBalanceIn: Dec,
  tokenWeightIn: Dec,
  tokenBalanceOut: Dec,
  tokenWeightOut: Dec,
  swapFee: Dec
): Dec {
  const number = tokenBalanceIn.quo(tokenWeightIn);
  const denom = tokenBalanceOut.quo(tokenWeightOut);
  const scale = oneDec.quo(oneDec.sub(swapFee));

  return number.quo(denom).mul(scale);
}

beforeSpotPriceInOverOut = WeightedPoolMath.calcSpotPrice(
      new Dec(inPoolAsset.amount),
      new Dec(inPoolAsset.weight),
      new Dec(outPoolAsset.amount),
      new Dec(outPoolAsset.weight),
      swapFee ?? this.swapFee
    );



 const effectivePrice = new Dec(tokenInAmount).quo(new Dec(tokenOut.amount));
    const priceImpact = effectivePrice
      .quo(beforeSpotPriceInOverOut)
      .sub(new Dec("1"));
```
# 오스모시스-타 토큰과의 교환비율
두 토큰이 있는 해당 유동성풀에 있는 두 토큰의 amount비율이 그것이다.

# Osmosis Api

- lcd 

- pool 정보 가져오는 api & swap fee도 
    - https://lcd.osmosis.zone/osmosis/gamm/v1beta1/pools
    - https://lcd-osmosis.keplr.app/osmosis/gamm/v1beta1/pools?pagination.limit=1000
- Estimate token_out_amount값 가져오기
    - https://lcd.osmosis.zone/osmosis/gamm/v1beta1/{pool_id}/estimate/swap_exact_amount_in
- swap fee 가져오는 api
    - https://lcd-osmosis.keplr.app/osmosis/gamm/v1beta1/pools/{pool_id}
api설명서 공식 홈피: https://docs.osmosis.zone/api?v=LCD#/operations/EstimateSwapExactAmountIn

# osmosis 프론트

    // const integer = weightRatio.truncate();
    // const fractional = minus(weightRatio,integer);

    // const integerPow = powInt(y, integer);

    // const fractionalPow = powApprox(base, fractional, powPrecision);

    // const www =  integerPow.mul(fractionalPow);

    // https://github.com/osmosis-labs/osmosis-frontend/blob/98abaeb2cd72666a63717bf468099190a49df9e3/packages/math/src/utils.ts#L8

- estimate들(estimateswapexactin도 포함)
    https://github.com/osmosis-labs/osmosis-frontend/blob/5372259aed83cbe99e261606eec84ce981418c57/packages/math/src/pool/estimates.ts 

    https://github.com/osmosis-labs/osmosis-frontend/blob/5372259aed83cbe99e261606eec84ce981418c57/packages/math/src/pool/estimates.ts#L357

# osmojs 패키지

- swapExactAmountIn
https://github.com/cosmology-tech/cosmology/tree/main/packages/core#lookuproutesfortrade

- cosmology(swap.tx)
    - https://github.com/cosmology-tech/cosmology/blob/2f80fdea0b3e846de6fd62f3d3c20da63c36e5bc/packages/cli/src/commands/swap.ts
## swap tx

{
    "account_number": "706372",
    "chain_id": "osmosis-1",
    "fee": {
        "amount": [
            {
                "denom": "uosmo",
                "amount": "625"
            }
        ],
        "gas": "250000"
    },
    "memo": "",
    "msgs": [
        {
            "type": "osmosis/gamm/swap-exact-amount-in",
            "value": {
                "routes": [
                    {
                        "pool_id": "2",
                        "token_out_denom": "uion"
                    }
                ],
                "sender": "osmo1aygdt8742gamxv8ca99wzh56ry4xw5s3dtgtpf",
                "token_in": {
                    "amount": "100000",
                    "denom": "uosmo"
                },
                "token_out_min_amount": "75"
            }
        }
    ],
    "sequence": "30"
}

{
    "account_number": "706372",
    "chain_id": "osmosis-1",
    "fee": {
        "amount": [
            {
                "amount": "240",
                "denom": "uosmo"
            }
        ],
        "gas": "95694"
    },
    "memo": "",
    "msgs": [
        {
            "type": "cosmos-sdk/MsgSend",
            "value": {
                "amount": [
                    {
                        "amount": "100000",
                        "denom": "uosmo"
                    }
                ],
                "from_address": "osmo1aygdt8742gamxv8ca99wzh56ry4xw5s3dtgtpf",
                "to_address": "osmo1q8cjl2udxxjft3anngtap3frpm9uhjxwxdtse7"
            }
        }
    ],
    "sequence": "30"
}

# Fee

유동성풀 제작자는 두가지 fee를 설정할 수 있는데

- swap fee: 스왑할 때 내는 수수료   
- exit fee: Lps들이 풀에서 자기들 에셋을 뺄 때 부과하는 수수료

- cosmolog패키지에서 fee 계산하는법
    보니까 그냥 단순히 이미 정의된 값을 쓰더라고

    코드: `const fee = FEES.osmosis.swapExactAmountIn('low'); // low, medium, high`

    FEES 정의: https://github.com/osmosis-labs/osmojs/blob/main/packages/osmojs/src/utils/gas/values.ts

Fees
In addition to liquidity mining, Osmosis provides three sources of revenue: transaction fees, swap fees, and exit fees.

TX Fees 

Transaction fees are paid by any user to post a transaction on the chain. The fee amount is determined by the computation and storage costs of the transaction. Minimum gas costs are determined by the proposer of a block in which the transaction is included. This transaction fee is distributed to OSMO stakers on the network.

Validators can choose which assets to accept for fees in the blocks that they propose. This optionality is a unique feature of Osmosis.

Swap Fees

Swap fees are fees charged for making a swap in an LP pool. The fee is paid by the trader in the form of the input asset. Pool creators specify the swap fee when establishing the pool. The total fee for a particular trade is calculated as percentage of swap size. Fees are added to the pool, effectively resulting in pro-rata distribution to LPs proportional to their share of the total pool.

Exit Fees

Osmosis LPs pay a small fee when withdrawing from the pool. Similar to swap fees, exit fees per pool are set by the pool creator.
Exit fees are paid in LP tokens. Users withdraw their tokens, minus a percent for the exit fee. These LP shares are burned, resulting in pro-rata distribution to remaining LPs.

# 용어

Incentivized Pool (보상 : OSMO )    
MED-OSMO, MED-ATOM 풀에 LP를 예치하면, OSMO를 보상으로 받을 수 있습니다. 현재 메디블록은 Incentivized Pool에 등록이 될 수 있도록 오스모시스에 프로포절을 등록하였습니다. 정상적으로 오스모시스에 Incentivized Pool 오픈 시, 공지로 안내해 드릴 예정입니다.  

External Incentivize Pool (보상 : OSMO, MED)    
MED-OSMO, MED-ATOM 풀에 LP를 예치하면, OSMO뿐만 아니라 MED를 추가적으로 받을 수 있습니다. 이 풀은 메디블록측에서 오스모시스의 유동성을 확보하기 위하여 마련한 기간 한정 풀입니다. 이 풀은 Incentivized Pool 에 등록이 정상적으로 완료된 이후, 신규 프로포절 등록 및 승인 이후에 가능해집니다. 정상적으로 External Incentivize Pool 오픈 시, 공지로 안내해 드릴 예정입니다.

참조: https://blog.medibloc.org/timeline/notice/18293