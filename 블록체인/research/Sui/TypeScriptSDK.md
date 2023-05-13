# Sui Network과 통신

`JsonRpcProvider`클래스로 서버와 통신가능 - rpc서버 url - local: http://127.0.0.1:9000 - DevNet: https://fullnode.devnet.sui.io

커스텀 커넥션도 가능 custom connections, with your own URLs to your fullnode and faucet server

```ts
import { JsonRpcProvider, Connection } from "@mysten/sui.js";
// Construct your connection:
const connection = new Connection({
  fullnode: "https://fullnode.devnet.sui.io",
  faucet: "https://faucet.devnet.sui.io/gas",
});
// connect to a custom RPC server
const provider = new JsonRpcProvider(connection);
// get tokens from a custom faucet server
await provider.requestSuiFromFaucet(
  "0xcc2bd176a478baea9a0de7a24cd927661cc6e860d5bacecb9a138ef20dbab231"
);
```

tx 튜토리얼
https://docs.sui.io/build/prog-trans-ts-sdk

# tx 생성 후 post

```ts
import {
  Ed25519Keypair,
  JsonRpcProvider,
  RawSigner,
  TransactionBlock,
} from "@mysten/sui.js";
// Generate a new Ed25519 Keypair
const keypair = new Ed25519Keypair();
const provider = new JsonRpcProvider();
const signer = new RawSigner(keypair, provider);
const tx = new TransactionBlock();
tx.transferObjects(
  [
    tx.object(
      "0xe19739da1a701eadc21683c5b127e62b553e833e8a15a4f292f4f48b4afea3f2"
    ),
  ],
  tx.pure("0x1d20dcdb2bca4f508ea9613994683eb4e76e9c4ed371169677c1be02aaf0b12a")
);
const result = await signer.signAndExecuteTransactionBlock({
  transactionBlock: tx,
});
console.log({ result });
```

1000MIST를 send하는 과정

```ts
import {
  Ed25519Keypair,
  JsonRpcProvider,
  RawSigner,
  TransactionBlock,
} from "@mysten/sui.js";
// Generate a new Keypair
const keypair = new Ed25519Keypair();
const provider = new JsonRpcProvider();
const signer = new RawSigner(keypair, provider);
const tx = new TransactionBlock();
const [coin] = tx.splitCoins(tx.gas, tx.pure(1000));
tx.transferObjects([coin], tx.pure(keypair.getPublicKey().toSuiAddress()));
const result = await signer.signAndExecuteTransactionBlock({
  transactionBlock: tx,
});
console.log({ result });
```

깃허브 주소: https://github.com/MystenLabs/sui/tree/main/sdk/typescript

필드 설명: http://typescript-sdk-docs.s3-website-us-east-1.amazonaws.com/classes/Connection.html

#

컨텍스트가 다를 때 그대로 TransactionBlock 객체를 넘기면 안넘어감 => 직렬화 해서 넘겨야 받아올 수 있음
TransactionBlock.from()안에 스트링으로 serialize()된 값을 넣으면됨

# Trasaction 클래스 api

Sui supports following transactions:

txb.splitCoins(coin, amounts) - Creates new coins with the defined amounts, split from the provided coin. Returns the coins so that it can be used in subsequent transactions.
Example: txb.splitCoins(txb.gas, [txb.pure(100), txb.pure(200)])
txb.mergeCoins(destinationCoin, sourceCoins) - Merges the sourceCoins into the destinationCoin.
Example: txb.mergeCoins(txb.object(coin1), [txb.object(coin2), txb.object(coin3)])
txb.transferObjects(objects, address) - Transfers a list of objects to the specified address.
Example: txb.transferObjects([txb.object(thing1), txb.object(thing2)], txb.pure(myAddress))
txb.moveCall({ target, arguments, typeArguments }) - Executes a Move call. Returns whatever the Sui Move call returns.
Example: txb.moveCall({ target: '0x2::devnet_nft::mint', arguments: [txb.pure(name), txb.pure(description), txb.pure(image)] })
txb.makeMoveVec({ type, objects }) - Constructs a vector of objects that can be passed into a moveCall. This is required as there’s no way to define a vector as an input.
Example: txb.makeMoveVec({ objects: [txb.object(id1), txb.object(id2)] })
txb.publish(modules, dependencies) - Publishes a Move package. Returns the upgrade capability object.

https://docs.sui.io/devnet/build/prog-trans-ts-sdk#passing-transaction-results-as-arguments
