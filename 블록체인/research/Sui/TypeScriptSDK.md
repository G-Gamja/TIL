# Sui Network과 통신

`JsonRpcProvider`클래스로 서버와 통신가능
    - rpc서버 url
        - local: http://127.0.0.1:9000
        - DevNet: https://fullnode.devnet.sui.io

커스텀 커넥션도 가능 custom connections, with your own URLs to your fullnode and faucet server
```ts
import { JsonRpcProvider, Connection } from '@mysten/sui.js';
// Construct your connection:
const connection = new Connection({
  fullnode: 'https://fullnode.devnet.sui.io',
  faucet: 'https://faucet.devnet.sui.io/gas',
});
// connect to a custom RPC server
const provider = new JsonRpcProvider(connection);
// get tokens from a custom faucet server
await provider.requestSuiFromFaucet(
  '0xcc2bd176a478baea9a0de7a24cd927661cc6e860d5bacecb9a138ef20dbab231',
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
} from '@mysten/sui.js';
// Generate a new Ed25519 Keypair
const keypair = new Ed25519Keypair();
const provider = new JsonRpcProvider();
const signer = new RawSigner(keypair, provider);
const tx = new TransactionBlock();
tx.transferObjects(
  [tx.object('0xe19739da1a701eadc21683c5b127e62b553e833e8a15a4f292f4f48b4afea3f2')],
  tx.pure('0x1d20dcdb2bca4f508ea9613994683eb4e76e9c4ed371169677c1be02aaf0b12a'),
);
const result = await signer.signAndExecuteTransactionBlock({ transactionBlock: tx });
console.log({ result });
```


1000MIST를 send하는 과정
```ts
import {
  Ed25519Keypair,
  JsonRpcProvider,
  RawSigner,
  TransactionBlock,
} from '@mysten/sui.js';
// Generate a new Keypair
const keypair = new Ed25519Keypair();
const provider = new JsonRpcProvider();
const signer = new RawSigner(keypair, provider);
const tx = new TransactionBlock();
const [coin] = tx.splitCoins(tx.gas, tx.pure(1000));
tx.transferObjects([coin], tx.pure(keypair.getPublicKey().toSuiAddress()));
const result = await signer.signAndExecuteTransactionBlock({ transactionBlock: tx });
console.log({ result });
```
깃허브 주소: https://github.com/MystenLabs/sui/tree/main/sdk/typescript

필드 설명: http://typescript-sdk-docs.s3-website-us-east-1.amazonaws.com/classes/Connection.html