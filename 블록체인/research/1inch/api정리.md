- 1inch api docs: https://docs.1inch.io/docs/aggregation-protocol/api/swap-params/

- 1inch api swagger: https://api.1inch.exchange/swagger/ethereum/#/Swap/ExchangeController_getSwap

# swap과정
바꿔먹을 코인들 컨트랙 주소 알아내고 -> 1inch router가 allowance 얻었는지 확인한 후(approve/allowance) -> (allowance가 안되었다면 approve/transaction)이 api써서 call data
만든 후에 이걸로 tx날리고 allowance를 뚫는다(1inch사이트에서는 permit and swap이라고 나온다 처음 스왑하는 케이스(from토큰만 뚫려도됨)일 경우)
-> 스왑 루트 모니터링(quote)(안해도 됨) -> 스왑(swap)통해서 1inch 라우터 스마트 컨트렉에 필요한 call data를 얻을 수 있음 이걸 rawTx의 data필드에 넣어주면 됨

- Lookup addresses of tokens you want to swap, for example ‘0xxx’ , ‘0xxxx’ for DAI -> 1INCH

- Check for allowance of 1inch router contract to spend source asset (/approve/allowance)

- If necessary, give approval for 1inch router to spend source token (/approve/transaction)

- Monitor the best exchange route using (/quote)
 - The "quote" in 1inch network refers to the estimated price of a token in terms of the desired denomination token. This quote is used to determine the cost of a trade and helps ensure that the trade is executed at the desired price.

- When you ready use to perform swap (/swap)

# spender 주소 api
- GET
- https://api.1inch.io/v5.0/1/approve/spender
- RETURN
- 
```json
{
  "address": "0x1111111254eeb25477b68fb85ed929f73a960582"
    }
 ``` 
# swap params

slippage: 0~50으로 설정할 것

# 오류 메시지 종류

swap시 다음의 이유로 오류를 뱉어낼 수 있음
-   Insufficient liquidity
-   Cannot estimate
-   You may not have enough ETH balance for gas fee
-   FromTokenAddress cannot be equals to toTokenAddress
-   Cannot estimate. Don't forget about miner fee. Try to leave the buffer of ETH for gas
-   Not enough balance
-   Not enough allowance