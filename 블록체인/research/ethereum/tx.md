Transactions, which change the state of the EVM, need to be broadcast to the whole network. Any node can broadcast a request for a transaction to be executed on the EVM; after this happens, a validator will execute the transaction and propagate the resulting state change to the rest of the network.

Transactions require a fee and must be included in a validated block. To make this overview simpler we'll cover gas fees and validation elsewhere.

A submitted transaction includes the following information:

recipient – the receiving address (if an externally-owned account, the transaction will transfer value. If a contract account, the transaction will execute the contract code)
signature – the identifier of the sender. This is generated when the sender's private key signs the transaction and confirms the sender has authorized this transaction
nonce - a sequentially incrementing counter which indicates the transaction number from the account
value – amount of ETH to transfer from sender to recipient (in WEI, a denomination of ETH)
data – optional field to include arbitrary data
gasLimit – the maximum amount of gas units that can be consumed by the transaction. Units of gas represent computational steps
maxPriorityFeePerGas - the maximum price of the consumed gas to be included as a tip to the validator
maxFeePerGas - the maximum fee per unit of gas willing to be paid for the transaction (inclusive of baseFeePerGas and maxPriorityFeePerGas)

# tx sign 진행 순서
이더리움 트랜잭션 서명은 다음 순서로 진핸된다.

- nonce, gasPrice, gasLimit, to, value, data, v, r, s의 9개 필드를 포함하는 트랜잭션 데이터 구조를 만든다.
- 트랜잭션을 RLP 인코딩하여 serialize한다.
- 이 serialize 메시지의 Keccak256 해시를 계산한다.
- EOA의 개인키로 해시를 ECDSA 서명한다.
- ECDSA 서명의 계산된 r과 s값을 트랜잭션에 삽입한다.

- 트랜잭션에서 가장 중요한 "payload"는 value와 data라는 두 개의 필드에 의해 사용된다. 트랜잭션은 다음과 같이 네 가지 조합이 가능하다. 'value와 data 둘다 사용', 'value만 사용', 'data만 사용', 'value와 data 둘다 없음'.

- value만 있는 트랜잭션은 송금(payment) 이다. 그리고 데이터만 있는 트랜잭션은 컨트랙트 호출(invocation)이다. value도 data도 없는 트랜잭션은 그냥 가스 낭비다.

# tx object
transactionObject 는 다음과 같이 구성된다.
```ts
var transactionObject = {
    nonce: transactionCount, // the count of the number of outgoing transactions, starting with 0
    gasPrice: gasPrice, //  the price to determine the amount of ether the transaction will cost
    gasLimit: gasLimit, // the maximum gas that is allowed to be spent to process the transaction
    to: toAddress,
    from: ownerAddress,
    data: data, // could be an arbitrary message or function call to a contract or code to create a contract
    value: wei  // the amount of ether to send
};
```
# tx send 방식

만들어진 transactionObject 를 전송하는 방법은 `sendTransaction과` `sendRawTransaction으로` 나누어 볼 수 있다. 
두 방법의 차이점은 서명 방법의 차이로 `sendTransaction` 은 `transactionObject` 를 node에 보내면 node에서 서명을 하고 

`sendRawTransaction` 은 보낼 때 서명을 해서 보낸다. `sendTransaction` 을 하기 위해서는 node 에 Account 가 등록되어 
있어야 하고 `sendRawTransaction` 하기 위해서는 privateKey 를 알고 있어야 한다.

## sendRawTransaction

sendRawTransaction
transactionObject 를 만들 때, 파라미터에 해당하는 값들은 Hex 로 변환해서 입력한다. 
`ethereumjs-tx `를 사용해 privateKey transaction 을 서명해서 전송한다.

```ts
var Tx = require('ethereumjs-tx');

var rawTx = {
    nonce: web3.toHex(nonce), // the count of the number of outgoing transactions, starting with 0/ 송신자에 의해 보내진 트랜잭션의 갯수
    gasPrice: web3.toHex(gasPrice), //  the price to determine the amount of ether the transaction will cost / 트랜잭션이 실행될 때 발신자가 지불할 의향이 있는 가스의 가격을 wei로 표현한 값
    gasLimit: web3.toHex(gasLimit), // the maximum gas that is allowed to be spent to process the transaction / 트랜잭션 발신자가 트랜잭션이 실행될 때 트랜잭션의 가스 소모량의 최대값
    to: toAddress,
    from: ownerAddress,
    data: '0x00', // could be an arbitrary message or function call to a contract or code to create a contract / 메시지 콜에서만 존재 , 메시지 콜의 입력 데이터(아규먼트)
    value: web3.toHex(value)  // the amount of ether to send / 발신자 -> 수신자로 전달되는 wei의 양
};

var tx = new Tx(rawTx);
tx.sign(privateKey);

var serializedTx = tx.serialize();
var transactionHash = web3.eth.sendRawTransaction('0x' + serializedTx.toString('hex'));
```
```json
{
  from: "0xEA674fdDe714fd979de3EdF0F56AA9716B898ec8",
  to: "0xac03bb73b6a9e108530aff4df5077c2b3d481e5a",
  gasLimit: "21000",
  maxFeePerGas: "300",
  maxPriorityFeePerGas: "10",
  nonce: "0",
  value: "10000000000"
}
```

json-rpc call
```json
{
  "id": 2,
  "jsonrpc": "2.0",
  "method": "account_signTransaction",
  "params": [
    {
      "from": "0x1923f626bb8dc025849e00f99c25fe2b2f7fb0db",
      "gas": "0x55555",
      "maxFeePerGas": "0x1234",
      "maxPriorityFeePerGas": "0x1234",
      "input": "0xabcd",
      "nonce": "0x0",
      "to": "0x07a565b7ed7d7a678680a4c162885bedbb695fe0",
      "value": "0x1234"
    }
  ]
}
```
response 는 16진수로 값이 날라오는데 아래의 코드를 통해서 스트링값으로 받아올 수  있음
  ```tsx 
  // data.result는 16진수
  const totalAmount = BigInt(data?.result || '0').toString();
  ```
```json
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sendRawTransaction","params":[{0x어쩌구저쩌구tx를싸인하고나온값}],"id":1}'
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}

```

참조: https://ethereum.org/ko/developers/docs/transactions/

tx구조 톺기: https://steemit.com/busy/@anpigon/ethereum-3


# tx 안의 data 파라미터 값 생성하는 법

- smart contract의 method를 통해 keccak256해쉬값을 알아낸다.
- 그 값중 4byte를 발췌한다.
- Input 파라미터가 있다면 각 타입에 맞게 그 값을 Hash 변환하여 붙여준다.
- 2번에서 작업한 Hash값과 3번에서 작업한 Hash값을 붙여준다.

https://ethereum.org/ko/developers/docs/transactions/#the-data-field
