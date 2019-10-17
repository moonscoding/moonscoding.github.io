---
typora-root-url: ../../../moonscoding.github.io
---







## Docker

컨테이너 기반의 운영환경을 지원 

- 크로스 플랫폼 환경에서 호환성 유지에 유리
- 도커 파일을 이용해서 현상 유지에 유리 ( 시간이 지나도 같은 서버 환경을 구축할 수 있음 )



## Config

### install & uninstall

install

```shell
$ curl -fsSL https://get.docker.com/ | sh
```

```bash
$ yum install docker -y
```

uninstall

```shell
$ yum list installed | grep docker
```



### info

```shell
$ docker info
```



### version

```shell
$ docker version
```



### start & stop

start

```shell
$ service docker start
```

stop

```shell
$ service docker stop
```





## Image

### docker search

```shell
$ docker search <image>
```



### docker pull

```shell
$ docker pull <image>:<tag>
```



### docker images

```shell
$ docker images
$ docker images | grep <image>
```



### docker image

docker image ls [option] [reop]

- `--all`, `-a` 
  - show all images
- `--digests`
  - show digests
- `--filter`, `-f`
  - filter output based on conditions provided
- `--format`
  - pretty-print images using a go template
- `--no-trunc`
  - don't truncate output
- `--quiet`, `-q`
  - only show numeric IDs

```shell
$ docker image ls --all <image>
```



## Container

### docker run

- `-it` 
- `-d`
- `-p`
- `--name`

```shell
$ sudo docker run -d --name <name> -p <os_port>:<container_port> <image>
```



### docker log



## Docker File

Docker Image 생성을 위한 명령어 집합 파일



### Tutorial Dockerfile

```dockerfile
# 이미지
FROM <image>:<tag>

# 관리자 
MAINTAINER <username> <<useremail>>

# ARG - 호출방법 $<parameter> 
ARG <paramemer>

# WORKDIR
WORKDIR <path>

# ADD - 복사 ( host_path 현재위치 -> Dockerfile )
ADD <host_path> <container_path>

# COPY
COPY <host_path> <container_path>

# RUN - to container
RUN <command> 

# ENV
ENV NODE_ENV <profile>

# PORT
EXPOSE <host_port_A> <host_port_B>

# CMD
CMD ["npm", "start"]
```



NodeJS Sample

```dockerfile
# 이미지
FROM node:6.2.2

# 관리자 
MAINTAINER SuperMoon <jm921106@gmail.com>

# /app 디렉토리 생성
RUN mkdir -p /app

# /app 디덱토리를 WORKDIR로 설정
WORKDIR /app

# /현재 Dockerfile에 있는 경로의 모든 파일을 /app에 복사
ADD . /app

# npm install 실행
RUN npm install

# 환경변수 설정
ENV NODE_ENV development

# 포트 설정
EXPOSE 3000 80

# 컨테이너 실행 명령
CMD ["npm", "start"]
```



### docker build

- `-t`

```shell
$ docker build -t <repo>/<image>:<tag> <build_target_dir>
```





## Docker Compose





## Docker Swarn







> 참조
>
> - [https://docs.docker.com/engine/reference](https://docs.docker.com/engine/reference/commandline/image_ls/)
> - [https://programmingsummaries.tistory.com/](https://programmingsummaries.tistory.com/392?category=695325)