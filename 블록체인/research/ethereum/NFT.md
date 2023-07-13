# ERC_721

ERC721 ABI를 사용하여 특정 주소에서 발행된 NFT에 대한 정보를 가져오려면, 다음과 같은 단계를 따르면 됩니다.

1. Web3.js 라이브러리를 사용하여 Ethereum 클라이언트와 상호 작용합니다. 이를 위해서는 먼저 `web3.eth` 객체를 생성해야 합니다.

```javascript
const Web3 = require("web3");
const web3 = new Web3("https://mainnet.infura.io/v3/<your-infura-project-id>");
```

2. 해당 NFT가 발행된 스마트 컨트랙트의 ABI를 가져옵니다. 이것은 일반적으로 스마트 컨트랙트의 소스 코드에서 추출할 수 있는 JSON 파일입니다.

3. web3.js의 `Contract` 객체를 사용하여 스마트 컨트랙트 인스턴스를 생성합니다.

```javascript
const contractAddress = "0x..."; // 스마트 컨트랙트 주소
const contractABI = [
  /* 스마트 컨트랙트 ABI(JSON) */
];
const contract = new web3.eth.Contract(contractABI, contractAddress);
```

4. `balanceOf` 메서드를 호출하여 특정 주소가 발행한 NFT의 수를 가져옵니다. `balanceOf` 메서드는 ERC721 표준 메서드 중 하나로, 해당 주소가 발행한 NFT의 수를 반환합니다.

```javascript
const ownerAddress = "0x..."; // NFT 소유자 주소
const tokenCount = await contract.methods.balanceOf(ownerAddress).call();
```

5. `tokenOfOwnerByIndex` 메서드를 사용하여 특정 인덱스의 NFT ID를 가져옵니다. 이 메서드는 `ownerAddress` 주소의 `index` 번째 NFT ID를 반환합니다.

```javascript
const index = 0; // 가져올 NFT 인덱스
const tokenId = await contract.methods
  .tokenOfOwnerByIndex(ownerAddress, index)
  .call();
```

6. 마지막으로, `getApproved` 및 `getTokenMetadata`(또는 `tokenURI`) 메서드를 사용하여 토큰의 메타데이터를 가져옵니다. `getApproved` 메서드는 NFT 전송을 승인한 주소, `getTokenMetadata`(또는 `tokenURI`) 메서드는 NFT의 이름, 설명, 이미지 URL 등과 같은 상세한 정보를 반환합니다.

```javascript
const approvedAddress = await contract.methods.getApproved(tokenId).call();

const metadataUrl = await contract.methods.getTokenMetadata(tokenId).call(); // 또는
const metadataUrl = await contract.methods.tokenURI(tokenId).call(); // getTokenMetadata 대신 tokenURI를 사용할 수도 있습니다.

// metadataUrl에 HTTP GET 요청을 보내어 실제 JSON 형식의 메타데이터를 가져올 수 있습니다.
```

- balanceOf(owner: address) : 해당 주소의 소유한 토큰 수를 반환합니다.
- ownerOf(tokenId: number) : 해당 토큰 ID의 소유자 주소를 반환합니다.
- safeTransferFrom(from: address, to: address, tokenId: number, data?: string) : 해당 토큰을 안전하게 전송합니다. (ERC721의 safeTransfer 메서드)
- transferFrom(from: address, to: address, tokenId: number) : 해당 토큰을 전송합니다.
- approve(to: address, tokenId: number) : 해당 토큰에 대한 승인을 부여합니다.
- setApprovalForAll(operator: address, approved: boolean) : 주소 간에 모든 토큰에 대한 승인을 부여/취소합니다.
- getApproved(tokenId: number) : 해당 토큰의 승인된 주소를 반환합니다.
- isApprovedForAll(owner: address, operator: address) : 주소 간에 모든 토큰에 대한 승인 여부를 반환합니다.
  위와 같은 단계를 수행하면 특정 주

erc721_abi: https://github.com/MetaMask/metamask-eth-abis/blob/d0474308a288f9252597b7c93a3a8deaad19e1b2/src/abis/abiERC721.ts#L62

해당 인터페이스 관련 설명 : https://docs.openzeppelin.com/contracts/3.x/api/token/erc721

---

ERC721 또는 ERC1155 토큰의 경우, contract address와 token ID 만으로는 소유주를 직접 확인할 수 없습니다. 소유주 정보는 대개 토큰을 발행한 스마트 컨트랙트에서 관리됩니다. 따라서 추가적인 스마트 컨트랙트 메서드를 호출하여 해당 정보를 알아내야 합니다.

ERC721의 경우 `ownerOf` 함수를 사용하여 token ID에 대한 소유자 주소를 가져올 수 있습니다. 예를 들어:

```solidity
function ownerOf(uint256 tokenId) public view returns (address) {}
```

이 함수를 호출하면 해당 token ID의 소유자 주소를 반환합니다.

ERC1155의 경우 토큰 ID가 고유한 인스턴스 ID로 사용되므로, 계정 별 보유량을 추적하기 위해 사용되는 balances 매핑에서 소유자를 확인할 수 있습니다. `balanceOf` 함수를 사용하여 계정이 특정 token ID의 인스턴스를 보유하는지 여부를 확인할 수 있습니다. 예를 들어:

```solidity
function balanceOf(address account, uint256 id) public view returns (uint256) {}
```

이 함수를 호출하면 지정된 계정이 해당 token ID의 인스턴스를 보유하는 경우 해당 인스턴스 수가 반환됩니다.

# ERC 721 - ERC 1155 구분 짓기

네, `isERC721Token` 및 `isERC1155Token` 함수를 구현하는 데 사용할 수 있는 ERC-721 및 ERC-1155 토큰의 ABI(JSON 형식) 정보가 있으면 해당 정보를 활용하여 구현할 수 있습니다.

아래는 TypeScript로 ERC-721/ERC-1155 토큰을 구분할 수 있는 ABI(PDOs)에 의존한 함수 예시입니다.

```typescript
import { ethers } from "ethers";

const ERC721_ABI = [
  // ERC721 메소드 목록
];

const ERC1155_ABI = [
  // ERC1155 메소드 목록
];

async function isERC721Token(
  contractAddress: string,
  tokenId: number
): Promise<boolean> {
  const provider = new ethers.providers.JsonRpcProvider();
  const contract = new ethers.Contract(contractAddress, ERC721_ABI, provider);
  try {
    const owner = await contract.ownerOf(tokenId);
    return true; // 오류 없이 호출되면 ERC-721 토큰으로 간주
  } catch (e) {
    return false; // 오류가 발생하면 ERC-721 토큰이 아닌 것으로 간주
  }
}

async function isERC1155Token(
  contractAddress: string,
  tokenId: number,
  instanceId: number
): Promise<boolean> {
  const provider = new ethers.providers.JsonRpcProvider();
  const contract = new ethers.Contract(contractAddress, ERC1155_ABI, provider);
  try {
    const balance = await contract.balanceOf(instanceId, tokenId);
    return true; // 오류 없이 호출되면 ERC-1155 토큰으로 간주
  } catch (e) {
    return false; // 오류가 발생하면 ERC-1155 토큰이 아닌 것으로 간주
  }
}
```

위 함수에서는 `ethers.js` 패키지를 사용하여 이더리움 노드와 상호작용하는 방법을 보여줍니다.

각 함수는 해당 컨트랙트 주소 및 토큰 ID 또는 인스턴스 ID를 매개변수로 받고, 호출 결과 오류가 없으면 해당 토큰을 그에 따라 ERC-721 또는 ERC-1155 토큰으로 결정합니다.

```typescript
// ERC-721 Token
const contractAddress = "0x1234...5678";
const tokenId = 123;

if (await isERC721Token(contractAddress, tokenId)) {
  console.log("This is an ERC-721 Token.");
} else {
  console.log("This is not an ERC-721 Token.");
}

// ERC-1155 Token
const instanceId = 1;
const tokenId2 = 456;

if (await isERC1155Token(contractAddress, tokenId2, instanceId)) {
  console.log("This is an ERC-1155 Token.");
} else {
  console.log("This is not an ERC-1155 Token.");
}
```

따라서 위 함수들은 ERC-721 및 ERC-1155 토큰의 ABI(JSON 형식) 정보를 사용하여 해당 토큰이 어떤 타입인지 확인하기 위한 다른 방법입니다.

```ts
import { ethers } from "ethers";

const contractAddress = "0x123abc...";
const provider = new ethers.providers.JsonRpcProvider();

const contract = new ethers.Contract(contractAddress, erc721Abi, provider);
const supportInterfaceFnHash = "0x01ffc9a7"; // supportsInterface 함수의 signature hash

// ERC-721 토큰인지 확인
if (await contract.supportsInterface(supportInterfaceFnHash)) {
  console.log("This is an ERC-721 token.");
}

// ERC-1155 토큰인지 확인
const contract1155 = new ethers.Contract(contractAddress, erc1155Abi, provider);
const erc1155SupportInterfaceFnHash = "0xd9b67a26"; // supportsInterface 함수의 signature hash for ERC-1155
if (await contract1155.supportsInterface(erc1155SupportInterfaceFnHash)) {
  console.log("This is an ERC-1155 token.");
}
```

# ERC 1155

## 소유주 확인법

ERC-1155의 balanceOfBatch 메서드를 사용하면 주어진 \_ids와 \_owners에 대한 계정의 잔액을 가져올 수 있습니다. 이 방법으로 소유주를 확인할 수 있지만, 더 직관적인 방법은 ownerOf와 같은 특수 함수를 사용하는 것입니다.

ERC-721과는 달리 ERC-1155는 각 토큰 ID마다 자체적인 소유자가 있다는 개념이 없습니다. 대신, 각 NFT ID는 여러 계정에서 공유될 수 있으며 balanceOf 함수를 사용하여 각 계정이 해당 NFT ID를 몇 개 보유하고 있는지 확인할 수 있습니다.

따라서, ERC-1155에서는 ownerOf 함수 대신, balanceOf와 balanceOfBatch와 같은 다른 함수를 사용하여 소유자를 식별해야합니다.

# 마켓플레이스

Nftmarketplace

- https://blur.io/collections
- https://rarible.com/
- https://zora.co/
- https://www.nansen.ai/pro
- https://magiceden.io/
- https://www.niftygateway.com/
- https://www.binance.com/en/nft/home

Nftswap

- https://sudoswap.xyz/#/

Api 유료
https://docs.alchemy.com/reference/nft-api-quickstart
