# Uint8Array란

  버퍼(Buffer)란?

- 임시로 바이너리 데이터를 저장하기 위한 메모리 공간 혹은 바이너리 데이터 자체

 

2. ArrayBuffer란?

- 자바스크립트에서 구현된 버퍼이다.

- 고정된  크기의 메모리 공간에 바이너리 데이터를 저장하는 객체다.

 

3. ArrayBufferView란?

- ArrayBuffer에 저장된 바이너리 데이터에 접근하는 객체다.

- TypedArray, DataView 2개가 제공된다.

 

4. TypedArray란?

- ArrayBufferView의 한 종류이다.

- 배열 요소의 타입/크기를 개발자가 지정하여 생성할 수 있다.

- Uint8Array, Uint16Array, Float32Array 등이 있다.

 

5. ArrayBuffer와 TypedArray의 관계

- TypedArray는 ArrayBuffer에 저장된 바이너리 데이터를 이용해 만드는 배열이다.


# ArrayBuffer란?
ArrayBuffer란 개발자가 지정한 메모리 크기만큼의 바이너리 데이터를 저장하는 객체다.

 

단, ArrayBuffer는 메모리를 확보해 바이너리 데이터를 0으로 저장해두는 역할만 하며,

실제 데이터 접근은 별도로 제공되는 `ArrayBufferView(TypedArray:Uint8Array등)를` 통해서만 가능하다.

 

1. ArrayBuffer: 메모리 확보 및 데이터 생성(디폴트 0000...)

2. ArrayBufferView: 데이터 접근(읽기, 수정)

   ㄴ TypedArray: Uint8Array, Uint16Array, Float32Array, ...

   ㄴ DataView  


참조    
추천: https://curryyou.tistory.com/441

https://velog.io/@longroadhome/%EB%AA%A8%EB%8D%98JS-%EC%8B%AC%ED%99%94-%EB%B0%94%EC%9D%B4%EB%84%88%EB%A6%AC-%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%99%80-%ED%8C%8C%EC%9D%BC