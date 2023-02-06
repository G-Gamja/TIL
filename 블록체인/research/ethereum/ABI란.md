# ABI란

SmartContract 의 함수와 파라미터에 대한 MetaData를 정의해
Contract의 객체를 만들 수 있고 Contract의 함수를 호출할 수 있는 표준방법입니다.

- 스마트 컨트랙 내에는 바이트 코드로 함수등이 인코딩되어 있는데 이를 디코딩하기 위해서 정의된 인터페이스를 의미함

ABI는 컨트랙트 내의 함수를 호출하거나 컨트랙트로부터 데이터를 얻는 방법이다. 이더리움 스마트 컨트랙트는 이더리움 블록체인에 배포된 바이트코드다. 컨트랙트 내에 여러 개의 함수가 있을 수 있을 것이다. ABI는 컨트랙트 내의 어떤 함수를 호출할지를 지정하는데 필요하며, 우리가 생각했던 대로 함수가 데이터를 리턴한다는 것을 보장하기 위해 반드시 필요하다.

Application Binary Interface: 컨트랙트의 함수와 매개변수들을 JSON 형식으로 나타낸 리스트다. 스마트 컨트랙트의 함수를 이용하기 위해서는 ABI를 사용하여 함수의를 해시할 수 있어야 한다. 이렇게 하면 해당 함수를 호출하기 위해 필요한 EVM 바이트 코드를 생성할 수 있다. 그러고선 트랜잭션의 데이터 필드인 Td에 포함되고 EVM에 의해 해석(컨트랙트 주소에 맞춰 코드가 해석됨)된다.

예시
```ts
    const contract = new web3.eth.Contract(ERC20_ABI as AbiItem[], params.tokenAddress);
 
```

https://muyu.tistory.com/entry/Ethereum-ABI-%EC%9D%B8%EC%BD%94%EB%94%A9-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0