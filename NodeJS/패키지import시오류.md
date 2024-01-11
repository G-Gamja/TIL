- 패키지를 여러개 쓰다보면 한 패키지에서 dependency로 쓰던걸 따로 yarn add해서 쓸 때가 있는데
이때 노드 모듈의 캐시가 이상해져서 서로 같은건데 오류를 뱉어낼 때도 있고, 서로 버젼이 달라서 오류를 뱉어낼 수 있음.
    - 원래라면 `import { Coin, Dec, Int } from '@keplr-wallet/unit';`
    - 굉장히 잘못된 `import { Coin, Dec, Int } from '@osmosis-labs/math/node_modules/@keplr-wallet/unit';`
    - 이 방법은 패키지의 노드모듈에 의존하기에 유지보수성이 떨어짐, 타 개발자가 패키지 관리에 어려움을 겪을 수 있음
    - 나는 오스모시스랩에서 사용한 패키지 버젼을 맞추어 add해주었고

* 만약 오류를 뱉어낸다 -> 그냥 노드 모듈을 지우고 새로 yarn해라 
- $ rm -rf node_modules
- $ yarn

참조 (노드모듈 지우는 방법): https://velog.io/@heony/gitignore