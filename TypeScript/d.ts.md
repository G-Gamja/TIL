# d.ts란

패키지이름으로 새 타입을 강제로 주입시킬 수 있음. 예로 a,b공통적으로 사용하는 패키지가 있는데 둘이 다르다게 인식되어지면
이렇게 타입만 선언한게 우선적으로 사용되어져서
서로 다른 타입이라고 오류를 안뱉음
```ts
declare module 'ledger-cosmos-js' {
  export default class CosmosApp {
    constructor(transport?: import('@ledgerhq/hw-transport').default, scrambleKey?: string);

    async publicKey(path: number[]): Promise<PublicKey>;

    async getVersion(): Promise<GetVersion>;

    async sign(path: number[], message: Uint8Array): Promise<Sign>;
  }
}
```