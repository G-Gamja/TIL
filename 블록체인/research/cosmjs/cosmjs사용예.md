```ts
import { SigningStargateClient, coins } from "@cosmjs/stargate";
import { Secp256k1HdWallet } from "@cosmjs/launchpad";

async function sendTransaction() {
  const mnemonic = "your-mnemonic-phrase"; // 지갑의 니모닉 프레이즈
  const wallet = await Secp256k1HdWallet.fromMnemonic(mnemonic);

  const [{ address }] = await wallet.getAccounts();

  const client = await SigningStargateClient.connectWithSigner(
    "your-rpc-endpoint", // 코스모스 RPC 엔드포인트
    wallet
  );

  const recipientAddress = "recipient-address"; // 수신자 주소
  const amount = [coins(1000, "uatom")]; // 보낼 코인량 (여기서는 uatom 사용)

  const sendMsg = {
    type: "cosmos-sdk/MsgSend",
    value: {
      from_address: address,
      to_address: recipientAddress,
      amount: amount,
    },
  };

  const fee = {
    amount: coins(5000, "uatom"), // 수수료 설정
    gas: "200000", // 가스 설정
  };

  const txBody = {
    messages: [sendMsg],
    memo: "Test transaction",
    timeout_height: 0,
    extension_options: [],
    non_critical_extension_options: [],
  };

  const signedTx = await client.signTx(
    address,
    txBody,
    fee,
    "Test transaction"
  );

  const txResult = await client.broadcastTx(signedTx);

  console.log("Transaction hash:", txResult.transactionHash);
}

sendTransaction().catch((error) => {
  console.error("Error:", error);
});
```
