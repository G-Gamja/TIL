# json-rpc 메서드 docs
https://docs.infura.io/infura/networks/ethereum/json-rpc-methods

JSON-RPC*(Remote Procedure Call)

JSON으로 인코딩된 원격 프로시저 호출이다. 매우 간단한 프로토콜로서, 소량의 데이터 타입과 명령들만을 정의하고 있다. 다수의 호출이 서버로 전송되고 순서없이 응답되는 것을 허용한다. 

클라이언트에서 서버로 필요한 파라미터를 보내어 서버에서 선언된 프로시저를 동작시킨다.

엔드포인트 URL(하나)에서 모든 요청과 응답을 받는다.   

    - 전달되는 바디의 필드중에 컨트랙 주소만 바꿔서 데이터 날리는걸 생각하면 이해가 쉬움

RPC 를 Json 포맷으로 표현한 것      
2.0부터 Request-Response 뿐만 아니라 응답없는 Notification 지원한다.    
4계층 TCP 에서 동작하여 Http 프로토콜을 얹을 필요없이, Json 만 전달하여 동작 가능하다.  
더 가볍고 빠르다.

요청 안에 들어가야 할 것    
method - 호출될 메소드의 이름 문자열    
params - 정의된 메소드에 대한 파라미터로서 전달될 객체들의 배열 
id - 임의 타입의 값. 이것은 요청에 대해 대응되는 응답을 매치시킨다.     

예시
```json
{

    "jsonrpc": "2.0",  // 이거는 고정이라 바꿀 필요없음
    "method": "eth_feeHistory", // doc보고 필요한 메서드명을 파악할 것
    "params": [ // 메서드에 필요한 파라미터를 알맞게 넣을 것
        20, "pending", [10, 30, 50]
    ],
    "id": 0 // 이거는 메시지 주고받고 특정짓는 아이디값이라 아무 값으로 넣어도 상관없음
}
```

# HTTP 위에 얹는 Json-RPC 스타일
HTTP 프로토콜을 사용하되, 요청을 1개로 통일 
POST 의 Body 등으로 json 을 넘기는데, 이 때 JsonRPC 포맷을 따른다. (id, method, params)
이렇게 하면..   
컨트롤러 1개로 모든 요청을 받고, 메서드를 호출해서 응답을 할 수 있다.