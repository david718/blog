---
title: (1025)docker 활용한 prisma2 data access API 개발환경 setting
date: 2019-10-25 12:11:61
category: TIL
---

현재 git clone 으로 local 에 저장한 git repo의  
directory 안에는 아래와 같은 구성요소들이 있다

- data access API logic files
- Dockerfile
- docker-compose.yml

## data access API logic files

pirsma2 를 통해 database에 접근하며,  
database는 postgres를 사용한다  
local에서 API 를 개발하면 배포까지 자동으로 될 수 있도록  
즉, 개발환경, test환경, 배포환경이 같도록  
docker를 활용한다

개발 순서는 아래와 같다

1. prisma2 client 를 작성하여 data access API 를 만든다
2. API 개발이 완료되면 `docker build` 를 실행시켜(`Dockerfile`을 읽음) docker image 를 update 한다
3. update 된 docker image는 prisma2 즉, API 만 있으므로 `docker-compose up` script 를 실행한다 (`docker-compose.yml`을 읽음)
4. 그 결과 2번에서 빌드한 docker image(prisma2)와 postgres 의 docker image 를 compose 하여 docker container 2개가 생성된다
5. 따라서 local 에서 prisma2 로 작성한 data access API 가 postgres 와 잘 연결되어 동작하는지 test가 가능하다

## Dockerfile

docker file은 docker language로 작성되어 있다  
`docker build` 명령어를 실행하면 이 file을 참조해서  
docker image 를 build 한다

### 실제 코드

```Dockerfile
FROM node:lts-alpine

ENV LANG=C.UTF-8

# Here we install GNU libc (aka glibc) and set C.UTF-8 locale as default.

RUN ALPINE_GLIBC_BASE_URL="https://github.com/sgerrand/alpine-pkg-glibc/releases/download" &&
// 여기는 생략

WORKDIR /app
ADD package.json yarn.lock tsconfig.json prisma/ ./
RUN yarn

ADD . .

EXPOSE 5555
CMD yarn run prisma2 dev --no-prompt
```

### docker image build 과정

1. 임시 docker container 생성
2. 명령어 수행
3. docker image 로 저장
4. 임시 docker container 삭제
5. 새로 만든 image 기반 임시 docker container 생성
6. 명령어 수행
7. odcker image 로 저장
8. 임시 docker container 삭제
9. 마지막 CMD 실행할 때까지 반복

### Dockerfile 명령어 순서를 주의하자

위 실제코드에서 확인할 수 있듯이  
package.json 을 통해 yarn 으로 install 하는 것  
즉, `RUN yarn` 은 그 내용이 자주 바뀌지 않는다  
따라서 caching 을 해둔 것을 그대로 쓰기에 실행하지 않는다

만약 내용이 자주 바뀌는 `ADD . .` 을 `RUN yarn` 앞에 썼다면  
내용이 바뀌어 `ADD . .`을 실행할 때마다  
`RUN yarn` 도 함께 다시 실행한다

이러한 낭비를 막기위해 명령어 순서를 주의하여  
Dockerfile 을 작성해야 한다

## docker-compose.yml

docker-compose 는 docker 의 CLI 를  
정리하여 합쳐놓은 file이다  
여러개의 docker container 를  
모아서 실행하기 쉽도록 도와준다

### 실제 코드

```yml
version: '2'

services:
  app:
    build: .
    restart: always
    container_name: prisma_app
    env_file:
      - envs/app.env
    volumes:
      - /app/node_modules
    depends_on:
      - postgres
    ports:
      - 5555:5555

  postgres:
    image: postgres:${POSTGRES_VERSION}
    restart: always
    container_name: prisma_postgres
    env_file:
      - envs/postgres.env
    volumes:
      - /var/lib/postgresql/data
    expose:
      - 5432
```

코드를 간단하게 살펴보면

app 과 postgres 2개의 image 를 run 한다  
각 image 에 필요한 env_file 즉, 환경변수 설정과  
volumes 설정이 있고 depends_on 이 따로 있다  
또한 expose 로 port 를 밖으로 빼고  
ports 로 container 의 port 를 밖으로 빼고 있다

### docker-compose up

이를 통해 postgres image 를 container 에서 실행하여  
database 를 가상으로 실행할 수 있다  
또한 app image 를 실행하여 prisma2 를 실행한다  
이때 prisma2 는 depends_on postgres 이므로  
postgres 와 연결되어 있다  
즉, local 환경에서 docker 를 실행하여  
개발 및 test 를 진행할 수 있다

> 이때 docker 를 실행하였기에 배포 환경과 똑같다
