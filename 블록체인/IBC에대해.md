타 체인간 통신을 위해 만들어진 것

transfer-> receive-> acknowledge

ack는 받은 쪽에서 잘 받았다고 보낸쪽에 확인 tx날린것 

근데 타 체인에서 보낸쪽의 코인을 그대로 가지고 있으면 문제가 있으니까 atom -> ibc/dfasdfsad이런식으로 임의의 이름안에 담긴다.

source-channe:transfer 요청들의 큐 리스트 의 인덱스, channel-1 이게 큐 인덱스 값이다.

relayer라는 애는 이 요청큐를 계속 보고 있다가 밀려있는 요청 큐를 보고 이걸 타 체인으로 넘겨주는거

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