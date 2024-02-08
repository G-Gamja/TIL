# Cross env

운영 체제 간에 일관된 방식으로 환경 변수를 설정할 수 있게 해주는 패키지

# PM2

노드 js의 프로세스 관리자로, 노드 js 애플리케이션을 관리하고 모니터링하는 데 사용되는 오픈 소스 프로세스 관리자이다.

# Nodemon

# 문제점

1. packages의 각 레포의 모듈을 못가져오고 있음

- 예시: packages/api/src/main.ts에서 packages/shared의 모듈을 가져오지
  내가 한것

1. shared 모듈 빌드
2. api 모듈 빌드

- 우선 이걸 하고 yarn start가 가능해짐
- 이런 오류가 뜸

````zsh
  App name:api-management id:5 disconnected
  App [api-management:5] exited with code [1] via signal [SIGINT]
  App [api-management:5] will restart in 8614ms```
````

- pm2 kill로 프로세스를 우선 죽임
- pm2 log로 로그 확인
- EADDRINUSE라는 오류때문에 프론트엔드가 뜨지 않음
  - EADDRINUSE 오류는 이미 사용 중인 포트에 애플리케이션을 바인딩하려고 할 때 발생합니다. 여기서는 포트 3000이 이미 사용 중이라는 것을 알 수 있습니다.
  - https://velog.io/@goblin820/Node.js-EADDRINUSE-%EC%97%90%EB%9F%AC
- 이 오류가 발생
- Error: Could not find a production build in the '/Users/ahnsihun/Workspace/api-service/packages/management/.next' directory. Try building your app with 'next build' before starting the production server. https://nextjs.org/docs/messages/production-start-no-build-id

- 각 프로젝트 빌드 후 다시 스타트
- 이 오류가 발생

```zsh
0|api-service  | Error: connect ETIMEDOUT 148.113.165.92:6379
0|api-service  |     at TCPConnectWrap.afterConnect [as oncomplete] (node:net:1247:16)
```

-> 레디스 서버에서 문제가 발생한 것으로 추측됨
-> 레디스 서버를 실행시키지 않으면 생길 수 있는 문제로 생각함, 레디스 서버를 실행시키고 다시 시도해봐야함
=> 레디스 서버 로컬로 실행시켜보기

도커 파일, 도커 컴포즈 파일 리서치

- 도커파일
- 도커 이미지를 만들기 위한 설정 파일

- 도커 컴포즈 실행 시켜봄 - docker-compose up

  - 에러 발생
  - Cannot connect to the Docker daemon at unix:///Users/ahnsihun/.docker/run/docker.sock. Is the docker daemon running?
  - 도커 데몬이 실행이 안되고 있어서 생긴 문제
    -> 맥 & 윈도우 => 도커 데스크탑 실행
    -> 리눅스 => sudo systemctl start docker 또는 자동 시작 설정은 sudo systemctl enable docker

  - 도커 데몬 실행여부 확인은?
    -> 맥&윈도우 => docker info
    -> 리눅스 => sudo systemctl status docker

- 도커 파일 실행 시도해봄 - docker build
- docker build 실행 -> 오류 -> docker build . 경로 지정 후 다시 스크립트 실행 -> 이런 경로는 없다는 오류가 발생

```txt
docker build 명령은 빌드 컨텍스트(일반적으로 Dockerfile이 있는 디렉토리)를 인수로 받습니다. 그러나 Dockerfile의 이름이나 경로를 직접 지정하려면 -f 또는 --file 옵션을 사용해야 합니다.

따라서, Dockerfiles/service.Dockerfile을 빌드하려면 다음과 같이 명령을 실행해야 합니다:

docker build -f Dockerfiles/service.Dockerfile .

이 명령은 현재 디렉토리(.)를 빌드 컨텍스트로 사용하고, Dockerfiles/service.Dockerfile을 Dockerfile로 사용합니다.

또한, 이 명령을 실행하기 전에 Dockerfiles/service.Dockerfile이 실제로 존재하는지 확인해야 합니다. 이 파일이 없거나 경로가 잘못된 경우 위와 같은 오류가 발생할 수 있습니다.
```

오 성공함

- docker images로 이미지 확인 -> 이미지 이름을 지정안하고 빌드에서 none이라고 뜸 -> docker tag 83954de439c4(이미지 ID값) cosmostation/api-service:local-test
  -> 이러면
  REPOSITORY TAG IMAGE ID CREATED SIZE
  cosmostation/api-service local-test 83954de439c4 8 minutes ago 4.06GB
  이렇게 바뀜

TODO 이미지 빌드까지 했고 도커 컴포즈로 실행시키고 있는중 docker-compose up이거 쳐서 이어서 하면 됨
