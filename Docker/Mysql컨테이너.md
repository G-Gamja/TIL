https://hipopatamus.tistory.com/109

[도커 설치](https://jhlee-developer.tistory.com/entry/Docker-Docker-%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%8B%A4%EC%8A%B5-CLI)

설치 후 버젼 확인

```zsh
docker -v
```

mysql 이미지 설치

```zsh
docker pull mysql
// 버젼 지정: docker pull mysql:8.0.0

```

docker 이미지 확인

```zsh
docker images
```

컨테이너 생성 및 실행

```bash
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=<password> -d -p 3306:3306 mysql:latest
```

● --name <container_name> : <container_name> 이름의 컨테이너를 실행한다.

● -e : 컨테이너 내에서 사용할 환경변수를 설정

● -e MYSQL_ROOT_PASSWORD=<password> : MySQL의 root 권한의 비밀번호를 <password>로 설정한다.

● -d : detach 모드로 컨테이너가 실행된다. 컨테이너가 백그라운드로 실행된다고 보면 된다.

● -p <호스트 포트> <컨테이너 포트> : 호스트와 컨테이너의 포트를 연결한다

● mysql:latest : 컨테이너에 사용할 이미지

!cli 대신에 yml 파일을 이용해서 mysql을 실행할 수도 있습니다.

```yml
version: "3.3"
services:
  mysql:
    image: mysql:latest
    container_name: mysql-container
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: <password>
    ports:
      - 3306:3306
```

```bash
docker-compose up -d
```

컨테이너 목록 조회

```zsh
docker ps -a
```

컨테이너 환경에서 mysql 접속

```zsh
docker exec -it mysql-container(컨테이너명) bash
```

컨테이너의 mysql 접속

```zsh
// bash-4.4#으로 변경된 것 확인
mysql -u root -p
```

참조: https://jmlim.github.io/docker/2019/07/30/docker-mysql-setup/

docker images
