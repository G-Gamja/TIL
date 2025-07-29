---

`solc`와 `zksolc`는 모두 **Solidity 스마트 컨트랙트 컴파일러**입니다. 하지만 각각의 목적과 대상 환경에 차이가 있습니다.

---

### `solc` (Solidity Compiler)

`solc`는 **이더리움 및 기타 EVM(Ethereum Virtual Machine) 호환 블록체인**을 위한 공식 Solidity 컴파일러입니다.

- **역할:** 사람이 작성한 Solidity 코드를 EVM이 이해하고 실행할 수 있는 **바이트코드(bytecode)**로 변환하는 핵심 도구입니다. 이 바이트코드는 이더리움 메인넷, BNB Smart Chain, Polygon, Avalanche 등 대부분의 EVM 호환 체인에 배포될 수 있습니다.
- **개발:** 이더리움 재단에서 직접 개발하고 유지보수합니다.
- **특징:**
  - C++로 작성되어 있으며, `solcjs`는 이를 웹 어셈블리(WebAssembly)로 컴파일하여 JavaScript 환경에서 사용할 수 있도록 한 버전입니다.
  - 코드 최적화, ABI(Application Binary Interface) 생성, AST(Abstract Syntax Tree) 출력 등 다양한 기능을 제공합니다.
  - EVM의 표준 Opcode 집합을 사용하여 바이트코드를 생성합니다.

---

### `zksolc` (ZKsync Solidity Compiler)

`zksolc`는 **ZKsync Era 네트워크에 특화된 Solidity 컴파일러**입니다. ZKsync Era는 이더리움의 Layer 2 스케일링 솔루션으로, zk-rollup 기술을 사용하여 트랜잭션 처리량을 높입니다. ZKsync Era는 표준 EVM과 유사하지만, 영지식 증명(ZK-proof)을 생성하는 데 최적화된 **zkEVM (Zero-Knowledge EVM)**을 사용합니다.

- **역할:** Solidity 코드를 **zkEVM이 이해하고 실행할 수 있는 바이트코드**로 변환합니다. ZKsync Era의 zkEVM은 일반적인 EVM과 내부 구조 및 Opcode 처리 방식에서 차이가 있기 때문에, 일반 `solc`로 컴파일된 바이트코드를 직접 실행할 수 없습니다. `zksolc`는 이러한 zkEVM 환경에 맞게 코드를 컴파일합니다.
- **개발:** Matter Labs (ZKsync 개발사)에서 개발합니다.
- **작동 방식:**
  - `zksolc`는 독자적으로 작동하는 것이 아니라, **내부적으로 `solc`를 자식 프로세스(child process)로 호출**합니다.
  - `solc`가 Solidity 코드를 중간 표현(Intermediate Representation, IR) 형태로 변환하면, `zksolc`는 이 중간 표현을 받아서 ZKsync Era의 zkEVM에 특화된 바이트코드(zkEVM bytecode)로 최종 컴파일합니다.
  - 이는 ZKsync Era가 EVM 호환성을 유지하면서도 ZK 기술의 이점을 활용할 수 있도록 하는 중요한 부분입니다.
- **특징:**
  - LLVM 기반 컴파일러 도구 체인을 사용합니다.
  - zkEVM의 특정 확장 기능(예: `mimic call`)을 지원하며, 일부 EVM Opcode의 동작이 다를 수 있습니다.
  - `zksolc`와 `solc`로 컴파일된 동일한 Solidity 컨트랙트라도 최종 바이트코드와 그 해시는 서로 다릅니다.

---

### `solc`와 `zksolc`의 관계 요약

| 특징          | `solc`                              | `zksolc`                                                |
| :------------ | :---------------------------------- | :------------------------------------------------------ |
| **목적**      | 일반 EVM (이더리움 메인넷, L1 체인) | ZKsync Era의 zkEVM (L2 체인)                            |
| **입력**      | Solidity 소스 코드                  | Solidity 소스 코드                                      |
| **출력**      | EVM 바이트코드                      | zkEVM 바이트코드                                        |
| **기반 기술** | 이더리움 재단 개발, C++             | Matter Labs 개발, LLVM 기반, **내부적으로 `solc` 호출** |
| **호환성**    | 광범위한 EVM 호환 체인              | ZKsync Era에 특화                                       |

간단히 말해, `solc`는 표준 이더리움 블록체인을 위한 것이고, `zksolc`는 ZKsync Era의 특수한 영지식 증명 환경에 맞춰 코드를 컴파일하는 전용 도구입니다. `zksolc`는 `solc`의 기능을 활용하여 zkEVM 환경에 맞는 최종 바이트코드를 만들어낸다고 보시면 됩니다.
