### 
POST https://eth-mainnet.public.blastapi.io
content-type: application/json

{

    "jsonrpc": "2.0",  // 이거는 고정이라 바꿀 필요없음
    "method": "eth_feeHistory", // doc보고 필요한 메서드명을 파악할 것
    "params": [ // 메서드에 필요한 파라미터를 알맞게 넣을 것
        20, "pending", [10, 30, 50]
    ],
    "id": 0 // 이거는 메시지 주고받고 특정짓는 아이디값이라 아무 값으로 넣어도 상관없음
}

여기서 이제 https://docs.infura.io/infura/networks/ethereum/json-rpc-methods
- https://www.quicknode.com/docs/ethereum/eth_getBalance
json-rpc메서드 문서 보면서 메서드명이랑 알맞은 파라미터 전달해서 직접 통신해볼 수 있다.

예시2 - http로 날려보기 

POST https://rpc-evmos-app.cosmostation.io
content-type: application/json
```json
// http
POST https://rpc-evmos-app.cosmostation.io
content-type: application/json
{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": [
        "0xA9b4823ec2718Fb09BD5FC21323B2BE5DD0aDDf6","latest"
    ],
    "id": 1
}
```
- response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x1bc16d674ec80000"
}
```