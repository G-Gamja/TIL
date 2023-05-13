# sui_getCheckpoint

https://github.com/MystenLabs/sui/pull/8542


quick start md : https://github.com/MystenLabs/sui/blob/main/doc/src/build/json-rpc.md


# get balance
```tsx
export const getUserSuiBalance = async (walletAddress) => {
  const requestData = {
    method: 'suix_getBalance',
    jsonrpc: '2.0',
    id: 1,
    params: [walletAddress, '0x2::sui::SUI'],
  }

  const response = await axios.post(RPC_URL, requestData)

  if (response.status === 200) {
    if (response.data.hasOwnProperty('error')) {
      if (response.data.error.code === -32602) {
        return { status: 404, type: 'error', message: 'საფულის მისამართი არასწორია.' }
      }
    } else {
      return { status: 200, balance: response.data.result.totalBalance / 10 ** 9 }
    }
  } else {
    return { status: 404, type: 'error', message: 'API კავშირი ვერ მოხერხდა! ცადეთ მოგვიანებით.' }
  }
}

    const { data, isLoading, error, isError } = useObjectsOwnedByAddress(
        accountAddress,
        { options: { showType: true, showDisplay: true } }
    );
```