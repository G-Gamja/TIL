### HTTP와 HTTPS의 차이점

HTTP(HyperText Transfer Protocol)와 HTTPS(HyperText Transfer Protocol Secure)는 데이터 통신을 위한 프로토콜로, HTTPS는 보안 버전입니다.

## HTTP와 HTTPS의 차이

HTTP는 데이터를 평문으로 전송하기 때문에 보안에 취약합니다. 반면에 HTTPS는 SSL/TLS 프로토콜을 사용하여 데이터를 암호화하고 보안적으로 안전한 통신을 제공합니다.

### HTTP

- 데이터가 암호화되지 않아 보안에 취약함.
- 기본 포트: 80

### HTTPS

- SSL/TLS 프로토콜을 사용하여 데이터를 암호화함.
- 기본 포트: 443

### CORS(Cross-Origin Resource Sharing) 이해하기

CORS는 웹 브라우저에서 실행되는 스크립트가 다른 도메인의 자원에 접근할 때 발생하는 보안 정책입니다.

## CORS(Cross-Origin Resource Sharing)

CORS는 다른 도메인의 자원에 접근할 때 브라우저에서 적용되는 보안 정책입니다.

### Same-Origin Policy

- 브라우저는 기본적으로 같은 출처에서만 리소스에 접근을 허용함.

### CORS 해결책

- 서버에서 적절한 CORS 헤더를 설정하여 다른 도메인에서의 접근을 허용함.
- 예시: 서버 응답 헤더에 "Access-Control-Allow-Origin: \*" 추가

### WebSocket과 HTTP의 차이

WebSocket은 양방향 통신을 지원하는 프로토콜이며, HTTP는 단방향 통신을 위한 프로토콜입니다.

## WebSocket과 HTTP의 차이

WebSocket은 양방향 통신을 지원하며, HTTP는 단방향 통신을 위한 프로토콜입니다.

### HTTP

- 클라이언트가 요청을 보내면 서버가 응답을 보내는 단방향 통신.
- Request-Response 모델.

### WebSocket

- 클라이언트와 서버 간 양방향 통신을 지원.
- 전이중(Full-duplex) 통신.
- 초기에 한 번의 연결로 계속해서 데이터를 주고받을 수 있음.
