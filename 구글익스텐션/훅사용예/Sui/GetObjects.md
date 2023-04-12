```tsx
  const aa = useOwnedObjectsSWR({ address: currentAddress, provider });

  const objectIdList = useMemo(
    () => (objectsOwnedByAddress?.result && objectsOwnedByAddress.result.data.map((coin) => coin.data.objectId)) || [],
    [objectsOwnedByAddress?.result],
  );

  const aaa = useObjectsSWR({
    objectIds: objectIdList,
    provider,
    options: {
      showType: true,
      showContent: true,
      showOwner: true,
      showDisplay: true,
    },
  });

  console.log('🚀 ~ file: index.tsx:102 ~ Sui ~ aaa:', aaa);
```
```json
// aa
{
    "data": [
        {
            "data": {
                "objectId": "0xd9cd85ca945184a06b00ce55fe9bd33e83b91009ce0ca2cede726387d7a769c0",
                "version": "1856724",
                "digest": "DbiVFjqDiJPpafayKCFZwEXskMYwT9crxzLaLpPUWo1M"
            }
        }
    ],
    "nextCursor": "0xd9cd85ca945184a06b00ce55fe9bd33e83b91009ce0ca2cede726387d7a769c0",
    "hasNextPage": false
}
```

```json
// aa
{
    "data": [
        {
            "data": {
                "objectId": "0xd9cd85ca945184a06b00ce55fe9bd33e83b91009ce0ca2cede726387d7a769c0",
                "version": "1856724",
                "digest": "DbiVFjqDiJPpafayKCFZwEXskMYwT9crxzLaLpPUWo1M"
            }
        }
    ],
    "nextCursor": "0xd9cd85ca945184a06b00ce55fe9bd33e83b91009ce0ca2cede726387d7a769c0",
    "hasNextPage": false
}
```