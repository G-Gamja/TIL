REST(Representational State Trasfer)    

자원을 이름으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든것을 의미한다. 웹 상의 자료를 HTTP위에서 SOAP이나 쿠키를 통한 세션 트랙킹 같은 별도의 전송 계층 없이 전송하기 위한 아주 간단한 인터페이스를 말한다. HTTP URI를 통해 자원(Resource)을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미한다. 

특징 
HTTP(s) 프로토콜 위에서 동작하는 표준이다.

다른 리소스를 얻기 위해서 `다른 URL`에 접근해야한다.
    - json-rpc는 하나의 url로 쇼부가 가능함
    - 안에 파람으로 메서드명을 다르게 보내는 것임
REST는 요청을 보낼 때 여러 HTTP Method를 통해서 보낸다. 

REST 방식은 어떤 객체에 대해서 CRUD 작업을 하는 것에 설계 되었다. 

 

REST API(Application Programming Interface)
REST 기반으로 서비스 API를 구현한 것