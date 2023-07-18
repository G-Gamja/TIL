## swap route

- post

```json
{
  "amountIn": "1000000",
  "sourceAsset": {
    "denom": "uusdc",
    "chainId": "axelar-dojo-1"
  },
  "destAsset": {
    "denom": "uatom",
    "chainId": "cosmoshub-4"
  },
  "cumulativeAffiliateFeeBps": "0"
}
```

- response

\*\* hops는 체인간 중간다리 체인을 의미함

악셀러 (1 USDC) -> 코스모스(ATOM)

```json
{
  "preSwapHops": [
    {
      "port": "transfer",
      // 여기가 출발 체인
      "channel": "channel-78", // 악셀러 쪽 채널
      "chainId": "axelar-dojo-1",

      "pfmEnabled": false,

      // 도착 체인에서 들어온 코인에 해당되는 데놈
      "destDenom": "ibc/F082B65C88E4B6D5EF1DB243CDA1D331D002759E938A0F5CD3FFDC5D53B3E349" // 뉴트론 체인
    }
  ],
  "postSwapHops": [
    {
      "port": "transfer",

      // 출발 체인(뉴트론)
      "channel": "channel-1",
      "chainId": "neutron-1",
      "pfmEnabled": true,
      // 도착한 코스모스 체인에서 받을 코인 데놈
      "destDenom": "uatom"
    }
  ],
  // 사용될 체인들의 체인아이디
  "chainIds": ["axelar-dojo-1", "neutron-1", "cosmoshub-4"],
  "sourceAsset": {
    "denom": "uusdc",
    "chainId": "axelar-dojo-1"
  },
  "destAsset": {
    "denom": "uatom",
    "chainId": "cosmoshub-4"
  },
  "amountIn": "1000000",
  "userSwap": {
    "swapVenue": {
      "name": "neutron-astroport",
      "chainId": "neutron-1"
    },
    "swapOperations": [
      {
        "pool": "neutron17wg25zvvud2d27pnxjv76f5xk5tm5jfda4c7003valghrlju5u6q5dt83n",
        "denomIn": "ibc/F082B65C88E4B6D5EF1DB243CDA1D331D002759E938A0F5CD3FFDC5D53B3E349",
        "denomOut": "ibc/C4CFF46FD6DE35CA4CF4CE031E643C8FDC9BA4B99AE598E9B0ED98FE3A2319F9"
      }
    ]
  },
  "userSwapAmountOut": "39330",
  "feeSwap": {
    "swapVenue": {
      "name": "neutron-astroport",
      "chainId": "neutron-1"
    },
    "swapOperations": [
      {
        "pool": "neutron1l3gtxnwjuy65rzk63k352d52ad0f2sh89kgrqwczgt56jc8nmc3qh5kag3",
        "denomIn": "ibc/F082B65C88E4B6D5EF1DB243CDA1D331D002759E938A0F5CD3FFDC5D53B3E349",
        "denomOut": "untrn"
      }
    ],
    "swapAmountOut": "100000"
  },
  "swapChainId": "neutron-1",
  "totalAffiliateFee": "0"
}
```

# PFM이란

As we can see from the previous step, the Cosmos Hub is an intermediate chain where pfmEnabled = true. PFM (packet-forward middleware) is a feature included in some chains that detects incoming IBC packets that contain forwarding data in the IBC memo field and will automatically dispatch an IBC packet to the next destination.

---

이전 단계에서 확인할 수 있듯이, Cosmos Hub은 pfmEnabled = true인 중간 체인입니다. PFM(Packet-Forward Middleware)은 일부 체인에 포함된 기능으로, IBC 메모 필드에 전달 데이터가 포함된 수신 IBC 패킷을 감지하고 자동으로 다음 목적지로의 IBC 패킷을 전달합니다.

# Construct Message

```json
- post
{
  "preSwapHops": [
    {
      "port": "transfer",
      "channel": "channel-78",
      "chainId": "axelar-dojo-1",
      "pfmEnabled": false,
      "destDenom": "ibc/F082B65C88E4B6D5EF1DB243CDA1D331D002759E938A0F5CD3FFDC5D53B3E349"
    }
  ],
  "postSwapHops": [
    {
      "port": "transfer",
      "channel": "channel-1",
      "chainId": "neutron-1",
      "pfmEnabled": true,
      "destDenom": "uatom"
    }
  ],
  //
  "chainIdsToAddresses": {
    "axelar-dojo-1": "axelar1aw303vafwmljlzz7ww7l384qlvmgajy57fk3ca",
    "neutron-1": "neutron1wnvqf444vh7pzv9zw8xdenprrgt6xucync90h0",
    "cosmoshub-4": "cosmos1s6ap2nl33pf303kt0trfw3sem54thymyv6e3x9"
  },
  "sourceAsset": {
    "denom": "uusdc",
    "chainId": "axelar-dojo-1"
  },
  "destAsset": {
    "denom": "uatom",
    "chainId": "cosmoshub-4"
  },
  "amountIn": "1000000",
  "userSwap": {
    "swapVenue": {
      "name": "neutron-astroport",
      "chainId": "neutron-1"
    },
    "swapOperations": [
      {
        "pool": "neutron17wg25zvvud2d27pnxjv76f5xk5tm5jfda4c7003valghrlju5u6q5dt83n",
        "denomIn": "ibc/F082B65C88E4B6D5EF1DB243CDA1D331D002759E938A0F5CD3FFDC5D53B3E349",
        "denomOut": "ibc/C4CFF46FD6DE35CA4CF4CE031E643C8FDC9BA4B99AE598E9B0ED98FE3A2319F9"
      }
    ]
  },
  "userSwapAmountOut": "39330",
  "userSwapSlippageTolerancePercent": "3",
  "feeSwap": {
    "swapVenue": {
      "name": "neutron-astroport",
      "chainId": "neutron-1"
    },
    "swapOperations": [
      {
        "pool": "neutron1l3gtxnwjuy65rzk63k352d52ad0f2sh89kgrqwczgt56jc8nmc3qh5kag3",
        "denomIn": "ibc/F082B65C88E4B6D5EF1DB243CDA1D331D002759E938A0F5CD3FFDC5D53B3E349",
        "denomOut": "untrn"
      }
    ],
    "swapAmountOut": "100000"
  },
  "affiliates": [
    {
      "basisPointsFee": "100",
      "address": "neutron1wnvqf444vh7pzv9zw8xdenprrgt6xucync90h0"
    }
  ]
}
```

리턴값으로 나온 값은값은

```typescript
function convertSnakeCaseToCamelCase(obj) {
  if (typeof obj !== "object" || obj === null) {
    return obj;
  }

  if (Array.isArray(obj)) {
    return obj.map((item) => convertSnakeCaseToCamelCase(item));
  }

  const camelCaseObj = {};

  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      const camelCaseKey = key.replace(/_([a-z])/g, (match, p1) =>
        p1.toUpperCase()
      );
      camelCaseObj[camelCaseKey] = convertSnakeCaseToCamelCase(obj[key]);
    }
  }

  return camelCaseObj;
}

const msg = JSON.parse(response.requested[0].msg);

try {
  const tx = await client.signAndBroadcast(
    msg.sender,
    [
      {
        typeUrl: messages.requested[0].msgTypeUrl,
        value: {
          ...convertSnakeCaseToCamelCase(msg),
          timeoutHeight: {
            // SET TIMEOUT
            revisionHeight: Long.fromNumber(currentHeight).add(100),
            revisionNumber: Long.fromNumber(currentHeight).add(100),
          },
        },
      },
    ],
    "auto"
  );

  console.log(tx.transactionHash);
} catch (e) {
  console.log(e);
}
```

https://skip-protocol.notion.site/Skip-API-9c502e3297334bd9809e3db3e6d63420

## 디앱 요청 메시지

```json
{
  "account_number": "1456440",
  "auth_info_bytes": {
    "signer_infos": [
      {
        "public_key": {
          "type_url": "/cosmos.crypto.secp256k1.PubKey",
          "value": "CiECoqz7XBbK22JBjnUSeHnKSU6RmzmcWom5Psoizic2jhY="
        },
        "mode_info": {
          "single": {
            "mode": "SIGN_MODE_DIRECT"
          }
        },
        "sequence": "20"
      }
    ],
    "fee": {
      "amount": [
        {
          "denom": "uatom",
          "amount": "2884"
        }
      ],
      "gas_limit": "115339"
    }
  },
  "body_bytes": {
    "messages": [
      {
        "type_url": "/ibc.applications.transfer.v1.MsgTransfer",
        "value": "0a087472616e73666572120b6368616e6e656c2d3134311a0f0a057561746f6d1206313030303030222d636f736d6f733161796764743837343267616d78763863613939777a683536727934787735733339736d6d686d2a3f6f736d6f31717076753830796664306a6e6e683379746e3839363433383274797072717130656a6d7161617361336a323333737538617061736671746c657232003880d8d3e8cbb4b6b9174294037b227761736d223a7b22636f6e7472616374223a226f736d6f31717076753830796664306a6e6e683379746e3839363433383274797072717130656a6d7161617361336a323333737538617061736671746c6572222c226d7367223a7b22737761705f776974685f616374696f6e223a7b22737761705f6d7367223a7b22746f6b656e5f6f75745f6d696e5f616d6f756e74223a2231373236353039222c2270617468223a5b7b22706f6f6c5f6964223a2231222c22746f6b656e5f6f75745f64656e6f6d223a22756f736d6f227d5d7d2c2261667465725f737761705f616374696f6e223a7b226962635f7472616e73666572223a7b227265636569766572223a22636f736d6f733161796764743837343267616d78763863613939777a683536727934787735733339736d6d686d222c226368616e6e656c223a226368616e6e656c2d30227d7d2c226c6f63616c5f66616c6c6261636b5f61646472657373223a226f736d6f3161796764743837343267616d78763863613939777a6835367279347877357333647467747066227d7d7d7d"
      }
    ],
    "extension_options": [],
    "non_critical_extension_options": []
  },
  "chain_id": "cosmoshub-4"
}
```

두번째 요청?

```json
{
  "account_number": "1456440",
  "auth_info_bytes": {
    "signer_infos": [
      {
        "public_key": {
          "type_url": "/cosmos.crypto.secp256k1.PubKey",
          "value": "CiECoqz7XBbK22JBjnUSeHnKSU6RmzmcWom5Psoizic2jhY="
        },
        "mode_info": {
          "single": {
            "mode": "SIGN_MODE_DIRECT"
          }
        },
        "sequence": "21"
      }
    ],
    "fee": {
      "amount": [
        {
          "denom": "uatom",
          "amount": "2884"
        }
      ],
      "gas_limit": "115339"
    }
  },
  "body_bytes": {
    "messages": [
      {
        "type_url": "/ibc.applications.transfer.v1.MsgTransfer",
        "value": "0a087472616e73666572120b6368616e6e656c2d3134311a0f0a057561746f6d1206313030303030222d636f736d6f733161796764743837343267616d78763863613939777a683536727934787735733339736d6d686d2a3f6f736d6f31717076753830796664306a6e6e683379746e3839363433383274797072717130656a6d7161617361336a323333737538617061736671746c6572320038809a90f799b8b6b9174294037b227761736d223a7b22636f6e7472616374223a226f736d6f31717076753830796664306a6e6e683379746e3839363433383274797072717130656a6d7161617361336a323333737538617061736671746c6572222c226d7367223a7b22737761705f776974685f616374696f6e223a7b22737761705f6d7367223a7b22746f6b656e5f6f75745f6d696e5f616d6f756e74223a2231373236353039222c2270617468223a5b7b22706f6f6c5f6964223a2231222c22746f6b656e5f6f75745f64656e6f6d223a22756f736d6f227d5d7d2c2261667465725f737761705f616374696f6e223a7b226962635f7472616e73666572223a7b227265636569766572223a22636f736d6f733161796764743837343267616d78763863613939777a683536727934787735733339736d6d686d222c226368616e6e656c223a226368616e6e656c2d30227d7d2c226c6f63616c5f66616c6c6261636b5f61646472657373223a226f736d6f3161796764743837343267616d78763863613939777a6835367279347877357333647467747066227d7d7d7d"
      }
    ],
    "extension_options": [],
    "non_critical_extension_options": []
  },
  "chain_id": "cosmoshub-4"
}
```
