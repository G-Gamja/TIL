패키지 속에서 buffer패키지를 써서 웹팩 5에서는 버퍼를 알아서 폴리필 해주지 않아서 생기는 문제를 해결하기 위해선

1. 리액트 스크립트 버젼 낮추기

2. 웹팩 설정으로 폴리필 추가해주기
   - > cra로 프로젝트 생성한 케이스는 웹팩 설정 파일에 직접 접근이 안되기 때문데 react-rewired-app으로 오버라이드 해야한다

https://80000coding.oopy.io/086282eb-6e8b-4cb4-85e2-395461009d09

- cra(ts)로 설정한 프로젝트 웹팩 재설정
  https://velog.io/@kmlee95/CRA%EB%A1%9C-%EC%9B%B9%ED%8C%A9%EC%84%A4%EC%A0%95
