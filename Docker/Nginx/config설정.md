Docker 컨테이너로 생성한 Nginx의 설정을 수정하려면, 일반적으로 다음과 같은 방법을 사용할 수 있습니다:

Docker 볼륨을 사용하는 방법: 이 방법은 Docker 컨테이너를 실행할 때 -v 또는 --volume 옵션을 사용하여 호스트 시스템의 설정 파일을 컨테이너의 설정 파일 위치에 마운트하는 것입니다. 이렇게 하면 호스트 시스템에서 설정 파일을 직접 수정할 수 있습니다. 예를 들어:

```bash
docker run -v /path/to/nginx.conf:/etc/nginx/nginx.conf -p 80:80 -d nginx

docker run -d --name board-reverse-proxy-nginx -p 80:80 -v /Users/ahnsihun/Workspace/playground/raw-board/proxy/nginx.conf:/etc/nginx/nginx.conf:ro nginx

docker run -d --name board-reverse-proxy-nginx -p 80:80 nginx;
```

위의 명령은 호스트 시스템의 /path/to/nginx.conf 파일을 컨테이너의 /etc/nginx/nginx.conf 위치에 마운트합니다.

Dockerfile을 사용하여 이미지를 빌드하는 방법: 이 방법은 Dockerfile에 COPY 명령을 사용하여 설정 파일을 이미지에 포함시키는 것입니다. 이렇게 하면 이미지를 빌드할 때 설정 파일이 이미지에 복사되므로, 컨테이너를 실행할 때마다 동일한 설정을 사용할 수 있습니다. 예를 들어:

```Dockerfile
FROM nginx
COPY nginx.conf /etc/nginx/nginx.conf
```

위의 Dockerfile은 nginx.conf 파일을 이미지의 /etc/nginx/nginx.conf 위치에 복사합니다. 이 이미지를 빌드하려
면 다음 명령을 사용합니다:
docker build -t my-nginx .
그런 다음 이 이미지를 사용하여 컨테이너를 실행합니다:

docker run -p 80:80 -d my-nginx
이 두 가지 방법 중에서 가장 적합한 방법을 선택하면 됩니다. 설정이 자주 변경되지 않는다면 Dockerfile을 사용하는 것이 좋습니다. 반면에 설정을 자주 변경해야 한다면 Docker 볼륨을 사용하는 것이 좋습니다.

# 도커 볼륨으로 엔진엑스 설정하기

Docker 볼륨에 직접 파일을 생성하는 것은 Docker의 볼륨 관리 방식 때문에 직접적으로는 불가능합니다. 볼륨은 컨테이너와 호스트 사이에서 데이터를 공유하는 데 사용되며, 일반적으로 컨테이너에서 생성된 데이터를 호스트에 유지하는 데 사용됩니다.

그러나, 임시 컨테이너를 사용하여 볼륨에 파일을 생성하고, 그 파일을 수정하는 방법이 있습니다. 아래는 그 과정을 설명한 것입니다:

1. 먼저, `test-volume`라는 이름의 볼륨을 생성합니다:

```bash
docker volume create test-volume
```

2. 그런 다음, 임시 컨테이너를 생성하고, 그 컨테이너에 `test-volume` 볼륨을 마운트합니다. 이 컨테이너에서 `nginx.conf` 파일을 생성합니다:

```bash
docker run -v test-volume:/etc/nginx --name temp alpine sh -c "echo 'user  nginx;' > /etc/nginx/nginx.conf"
```

위의 명령은 `alpine` 이미지를 기반으로 하는 임시 컨테이너를 생성하고, 그 컨테이너에서 `nginx.conf` 파일을 생성합니다. 이 파일은 `user  nginx;`라는 내용을 포함합니다.

3. 이제 `test-volume` 볼륨에 `nginx.conf` 파일이 생성되었습니다. 이 파일을 수정하려면, 다른 컨테이너에서 이 볼륨을 마운트하고, 그 컨테이너에서 파일을 수정하면 됩니다.

참고로, 이 방법은 임시 컨테이너를 사용하여 볼륨에 파일을 생성하는 방법입니다. 이 방법은 볼륨에 파일을 직접 생성하거나 수정해야 하는 경우에만 사용해야 합니다. 일반적으로는 컨테이너에서 생성된 데이터를 볼륨에 저장하고, 그 데이터를 다른 컨테이너와 공유하는 데 볼륨을 사용합니다.
\

# 내가 한 방법

```bash
docker run -d --name board-reverse-proxy-nginx -p 80:80 -v /Users/ahnsihun/Workspace/playground/raw-board/proxy/nginx.conf:/etc/nginx/nginx.conf:ro nginx
```

이떄 컨피그 파일

```conf
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include         /etc/nginx/mime.types;
    default_type    application/octet-stream;

    server {
        listen 80 default_server;
        server_name localhost;

    location / {
        proxy_pass http://localhost:3000;
    }

    location /api {
        proxy_pass http://localhost:8080;
    }
    }
}
```

# 앗 엔진엑스랑 포워딩된 서버랑 연결이 안돼!

[Nginx] connect() failed (111: Connection refused) while connecting to upstream
https://narup.tistory.com/236

해결

```conf
user nginx;
worker_processes  auto;
error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;
events {
    worker_connections  1024;
}
http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    server {
        listen 80;

        location / {
            # Docker 컨테이너는 기본적으로 자체 네트워크 환경에서 실행됩니다. 이는 컨테이너가 호스트 시스템의 localhost 또는 127.0.0.1에 직접 접근할 수 없음을 의미합니다.
            # Docker 컨테이너에서 호스트 시스템에 접근하려면, 특별한 DNS 이름 host.docker.internal을 사용해야 합니다. 이 DNS 이름은 Docker for Windows와 Docker for Mac에서 사용할 수 있습니다.
            # 따라서, Nginx 설정에서 proxy_pass 지시자의 값을 http://localhost에서 http://host.docker.internal로 변경해야 합니다:

            proxy_pass http://host.docker.internal:3000/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }

        location /api/ {
            proxy_pass http://host.docker.internal:8080/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    keepalive_timeout  65;
    include /etc/nginx/conf.d/*.conf;
}
```

# config 파일을 수정하고 싶은데, 컨테이너를 재시작해야 하나?

```bash
docker exec -it board-reverse-proxy-nginx nginx -s reload
```

Docker 컨테이너의 설정을 변경하려면 컨테이너를 재시작해야 합니다. 그러나 컨테이너를 삭제하지 않고도 이를 수행할 수 있습니다.

먼저, 변경된 `nginx.conf` 파일을 사용하여 Nginx 설정을 다시 로드해야 합니다. 그런 다음 Docker 컨테이너를 재시작해야 합니다.

다음은 이를 수행하는 단계입니다:

1. 먼저, 실행 중인 Nginx 컨테이너에 대한 셸을 열어야 합니다. 이를 위해 다음 명령을 사용할 수 있습니다:

```bash
docker exec -it board-reverse-proxy-nginx /bin/bash
```

2. 이제 Nginx에게 설정을 다시 로드하라고 알릴 수 있습니다. 이를 위해 다음 명령을 사용할 수 있습니다:

```bash
nginx -s reload
```

3. 이제 컨테이너에서 나와 Docker 컨테이너를 재시작할 수 있습니다:

```bash
exit
docker restart board-reverse-proxy-nginx
```

이렇게 하면 변경된 `nginx.conf` 파일을 사용하여 Nginx 설정이 다시 로드되고, Docker 컨테이너가 재시작됩니다.
