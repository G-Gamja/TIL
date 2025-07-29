# CREATE와 CREATE2

이더리움 컨트랙트를 배포할때 CREATE,CREATE2를 사용할 수 있다.

이때 생성되는 컨트랙트 주소는 다음과 같은 방식으로 생성된다

- CREATE: keccak256(rlp([sender, nonce])) [12:]
  - sender = 배포 지갑 주소
  - nonce = 해당 배포 지갑 주소의 nonce 값
- CREATE2: keccak256( 0xff ++ address ++ salt ++ keccak256(init_code))[12:]
  - 0xff - 고정값으로 생성된 주소가 기존의 CREATE opcode를 사용하여 생성된 주소와의 충돌을 방지하는 역할을 합니다.
  - address - 배포 지갑 주소
  - salt - 작성자가 제공한 임의의 salt 값
  - keccak256(init_code) - 배포 중인 바이트코드의 해시

즉 체인간 같은 컨트랙 주소가 나올 수 있는 이유는
CREATE2로 컨트랙 생성 + 동일한 배포자 + 동일한 컨트랙 바이트 코드 + `솔트값`
이 솔트값에 따라서 배포자는 배포될 컨트랙의 주소를 예측이 가능하다.

# op code

Solidity 코드 (고급 언어)
↓ (컴파일)

Opcode 시퀀스 (EVM의 저수준 명령어로 구성된 논리적 흐름)
↓ (이진수 인코딩)

바이트코드 (블록체인에 저장되는 실제 이진 데이터)
↓ (EVM에 의해 로드 및 파싱)

Opcode 실행 (EVM이 바이트코드 내의 Opcode들을 하나씩 해석하고 처리)

바이트 코드
60806040526008600055348015601457600080fd5b50603f8060226000396000f3fe6080604052600080fdfea26469706673582212206c0b14387e23dca73af0f7e35c45eafa12422727271941b551af3573a43002ba64736f6c63430008110033

Opcode
PUSH1 0x80 PUSH1 0x40 MSTORE PUSH1 0x8 PUSH1 0x0 SSTORE CALLVALUE DUP1 ISZERO PUSH1 0x14 JUMPI PUSH1 0x0 DUP1 REVERT JUMPDEST POP PUSH1 0x3F DUP1 PUSH1 0x22 PUSH1 0x0 CODECOPY PUSH1 0x0 RETURN INVALID PUSH1 0x80 PUSH1 0x40 MSTORE PUSH1 0x0 DUP1 REVERT INVALID LOG2 PUSH5 0x6970667358 0x22 SLT KECCAK256 PUSH13 0xB14387E23DCA73AF0F7E35C45 0xEA STATICCALL SLT TIMESTAMP 0x27 0x27 0x27 NOT COINBASE 0xB5 MLOAD 0xAF CALLDATALOAD PUSH20 0xA43002BA64736F6C634300081100330000000000

참조

- https://tech-talks.tistory.com/entry/%EB%8F%99%EC%9D%BC%ED%95%9C-%EC%8A%A4%EB%A7%88%ED%8A%B8-%EC%BB%A8%ED%8A%B8%EB%9E%99%ED%8A%B8-%EC%A3%BC%EC%86%8C%EB%A1%9C-%EB%B0%B0%ED%8F%AC-CREATE2
- https://tech-talks.tistory.com/entry/EVM%EA%B3%BC-Ethereum-Opcode
