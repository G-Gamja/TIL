```
 const { signAndExecuteTransactionBlock } = useWalletKit();


import { useWalletKit } from '@mysten/wallet-kit';


const tx = new TransactionBlock();

   const stakeCoin = tx.splitCoins(tx.gas, [tx.pure(amount)]);

   tx.setGasBudget(gasBudget);

   tx.moveCall({
     target: '0x3::sui_system::request_add_stake',
     arguments: [tx.object(SUI_SYSTEM_STATE_OBJECT_ID), stakeCoin, tx.pure(validatorAddress)],
   });

   const validatorResponse = await signAndExecuteTransactionBlock({
     transactionBlock: tx,
     options: {
       showInput: true,
       showEffects: true,
       showEvents: true,
     },
   });

   return validatorResponse;
```
