- 하위 워크스페이스에서 설치한 패키지는 루트의 node_modules에 설치되며, 또 다른 하위 워크스페이스에서도 이를 사용할 수 있어 하위 워크스페이스 간 패키지가 동일한게 있다면 하나만 설치해서 사용하기 때문에 용량을 절약할 수 있다.

// NOTE 모노레포 구성 예시(레포도 있음)
https://spookyjelly.tistory.com/81

### npm 스크립트 실행

워크스페이스를 사용하면 패키지를 하나로 관리할 수 잇는 것 뿐만아니라 npm 스크립트를 한 곳에서 실행할 수 있다. 직접 워크스페이스 폴더로 이동해 실행하는 것보다 간편할 것이다.

```bash
npm start -w workspace-a
# workspace-a/package.json에 등록된 start 명령어를 실행한다.
```

```bash
npm start --workspaces
# 전체 워크스페이스의 start 명령어를 실행한다.
```

```bash
npm start --workspaces --if-present
# 전체 워크스페이스에서 start 명령어가 있는 것만 실행한다.
```

## yarn workspace(워크스페이스)

https://d2.naver.com/helloworld/7553804
