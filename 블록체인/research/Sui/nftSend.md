## kiosk nft send

```ts
export enum KioskTypes {
  SUI = "sui",
  ORIGINBYTE = "originByte",
}
```

```ts
export function getKioskIdFromOwnerCap(
  object: SuiObjectResponse | SuiObjectData
) {
  const objectData =
    "data" in object && object.data ? object.data : (object as SuiObjectData);
  const fields =
    objectData.content?.dataType === "moveObject"
      ? (objectData.content.fields as { for?: string; kiosk?: string })
      : null;
  return fields?.for ?? fields?.kiosk!;
}
 -------------------
const tx = new TransactionBlock();
const recipientKiosks = await client.getOwnedObjects({
  owner: to,
  options: { showContent: true },
  filter: { StructType: ORIGINBYTE_KIOSK_OWNER_TOKEN },
});
const recipientKiosk = recipientKiosks.data[0];
const recipientKioskId = recipientKiosk
  ? getKioskIdFromOwnerCap(recipientKiosk)
  : null;

if (recipientKioskId) {
  tx.moveCall({
    target: `${obPackageId}::ob_kiosk::p2p_transfer`,
    typeArguments: [objectType],
    arguments: [
      tx.object(kioskId),
      tx.object(recipientKioskId),
      tx.pure(objectId),
    ],
  });
} else {
  tx.moveCall({
    target: `${obPackageId}::ob_kiosk::p2p_transfer_and_create_target_kiosk`,
    typeArguments: [objectType],
    arguments: [tx.object(kioskId), tx.pure(to), tx.pure(objectId)],
  });
}
return signer.signAndExecuteTransactionBlock(
  {
    transactionBlock: tx,
    options: {
      showInput: true,
      showEffects: true,
      showEvents: true,
    },
  },
  clientIdentifier
);
```

```ts
transactionBlock.moveCall({
  // NOTE packageId 는 원본 객체의 타입 맨 앞
  target: `${packageId}::ob_kiosk::p2p_transfer_and_create_target_kiosk`,
  // NOTE 파싱된 객체의 type
  typeArguments: [object.type ?? ""],
  arguments: [
    // NOTE 원본 객체에서의 kioskId
    transactionBlock.object(kioskId),
    // NOTE 수신인 주소
    transactionBlock.pure(recipient),
    // NOTE 파싱된 객체의 옵젝ID
    transactionBlock.pure(object.objectId),
  ],
});
```

원본 소스코드

```ts
export function useTransferKioskItem({
  objectId,
  objectType,
}: {
  objectId: string;
  objectType?: string | null;
}) {
  const client = useSuiClient();
  const activeAccount = useActiveAccount();
  const signer = useSigner(activeAccount);
  const address = activeAccount?.address;

  const obPackageId = useFeatureValue(
    "kiosk-originbyte-packageid",
    ORIGINBYTE_PACKAGE_ID
  );
  const { data: kioskData } = useGetKioskContents(address);

  const objectData = useGetObject(objectId);

  return useMutation({
    mutationFn: async ({
      to,
      clientIdentifier,
    }: {
      to: string;
      clientIdentifier?: string;
    }) => {
      if (!to || !signer || !objectType) {
        throw new Error("Missing data");
      }

      const kioskId = kioskData?.lookup.get(objectId);
      const kiosk = kioskData?.kiosks.get(kioskId!);

      if (!kioskId || !kiosk) {
        throw new Error("Failed to find object in a kiosk");
      }

      if (
        kiosk.type === KioskTypes.SUI &&
        objectData?.data?.data?.type &&
        kiosk?.ownerCap
      ) {
        const tx = new TransactionBlock();
        // take item out of kiosk
        const obj = take(
          tx,
          objectData.data?.data?.type,
          kioskId,
          kiosk?.ownerCap,
          objectId
        );
        // transfer as usual
        tx.transferObjects([obj], tx.pure(to));
        return signer.signAndExecuteTransactionBlock(
          {
            transactionBlock: tx,
            options: {
              showInput: true,
              showEffects: true,
              showEvents: true,
            },
          },
          clientIdentifier
        );
      }

      if (
        kiosk.type === KioskTypes.ORIGINBYTE &&
        objectData?.data?.data?.type
      ) {
        const tx = new TransactionBlock();
        const recipientKiosks = await client.getOwnedObjects({
          owner: to,
          options: { showContent: true },
          filter: { StructType: ORIGINBYTE_KIOSK_OWNER_TOKEN },
        });
        const recipientKiosk = recipientKiosks.data[0];
        const recipientKioskId = recipientKiosk
          ? getKioskIdFromOwnerCap(recipientKiosk)
          : null;

        if (recipientKioskId) {
          tx.moveCall({
            target: `${obPackageId}::ob_kiosk::p2p_transfer`,
            typeArguments: [objectType],
            arguments: [
              tx.object(kioskId),
              tx.object(recipientKioskId),
              tx.pure(objectId),
            ],
          });
        } else {
          tx.moveCall({
            target: `${obPackageId}::ob_kiosk::p2p_transfer_and_create_target_kiosk`,
            typeArguments: [objectType],
            arguments: [tx.object(kioskId), tx.pure(to), tx.pure(objectId)],
          });
        }
        return signer.signAndExecuteTransactionBlock(
          {
            transactionBlock: tx,
            options: {
              showInput: true,
              showEffects: true,
              showEvents: true,
            },
          },
          clientIdentifier
        );
      }
      throw new Error("Failed to transfer object");
    },
  });
}
```
