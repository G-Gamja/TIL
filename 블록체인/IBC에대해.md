# IBC
===================

### 블록체인 간 통신



코스모스 생태계에 있는 존(Zone)들은 IBC 프로토콜을 통해 토큰을 교환합니다. IBC 프로토콜은 모든 블록체인이 ‘즉각적인 완결성’을 가진 텐더민트 기반이라는 전제를 바탕으로 합니다. 즉, IBC 프로토콜을 통해 존과 허브(Hub)가 연결되려면 둘 다 텐더민트 기반의 블록체인이어야 합니다. 참고로 각각의 존들이 서로 다른 존과 일일이 IBC 프로토콜로 연결될 필요는 없습니다. 여러 개의 존이 오직 하나의 허브(Hub)에만 연결이 되어 있다면 연결된 존들끼리 토큰의 교환이 가능해집니다.
* 여기서 존은 뭐 `오스모시스`, `주노`. 허브는 `코스모스`로 이해하면 될듯 하다.





타 체인간 통신을 위해 만들어진 것

transfer-> receive-> acknowledge

ack는 받은 쪽에서 잘 받았다고 보낸쪽에 확인 tx날린것 

근데 타 체인에서 보낸쪽의 코인을 그대로 가지고 있으면 문제가 있으니까 atom -> ibc/dfasdfsad이런식으로 임의의 이름안에 담긴다.

source-channe:transfer 요청들의 큐 리스트 의 인덱스, channel-1 이게 큐 인덱스 값이다.

relayer라는 애는 이 요청큐를 계속 보고 있다가 밀려있는 요청 큐를 보고 이걸 타 체인으로 넘겨주는거

IBC의 작동 원리는 매우 간단하다. 체인 A의 계정에서 10개의 아톰을 체인 B로 보내는 경우를 예로 들어보자.

*   추적 : 계속해서 체인 B는 체인 A의 헤더를 받으며 그 반대의 경우도 마찬가지이다. 이렇게 하면 각 체인이 다른 체인의 검증자를 추적 할 수 있다. 본질적으로 각 체인은 다른 체인의 라이트 클라이언트(light-client)를 실행한다.

*   동결 : IBC 전송이 시작되면, 아톰은 체인 A에 묶이게 된다. 묶인다는건 이제 그 코인을 못쓰게 동결,묶어버린다는 의미

*   증거 전달 : 그런 다음, 10개의 아톰이 묶였다는 증거가 체인 A에서 체인 B로 전달된다.


확인 : 이 증명은 체인 A의 헤더에 대해 체인 B에서 검증되고, 이것이 유효하다면 체인 B에 10개의 아톰 상품권이 생성된다.
체인 B에서 생성된 아톰은 `실제 아톰`이 아니다. 아톰은 체인 A에만 존재하기 때문이다. 그것들은 이 아톰들이 체인 A에서 `동결`되었다는 증거와 함께 체인 A에서 나온 아톰의 B에 대한 표현이다.

그러니까 진짜 체인 a의 코인이 b로 간게 아니라 a에 있는 코인을 못쓰게 묶어놓고 b로 묶은 만큼의 코인이 있다고 증거? 도박장의 코인과 같은 느낌으로 변환해서 받는 것



# ibc process

참고자료: https://tutorials.cosmos.network/academy/3-ibc/5-token-transfer.html

# ibc proto

참고자료: https://ibc.cosmos.network/main/ibc/proto-docs.html

Height: https://ibc.cosmos.network/main/ibc/proto-docs.html#height

https://ibc.cosmos.network/main/apps/transfer/state-transitions.html

메시지가 fail되는 경우: https://ibc.cosmos.network/main/apps/transfer/messages.html

# IBC에 대해 (한글)

참고자료: https://medium.com/decipher-media/ibc-research-1-cosmos-ibc-process-ede52f7d04d0

IBC Denom이 왜 해시값인가요?: https://tutorials.cosmos.network/tutorials/5-ibc-dev/

한국 코스모스 코리아 블로그(텐더민트와 IBC): https://blog.naver.com/lunagram/221402006282