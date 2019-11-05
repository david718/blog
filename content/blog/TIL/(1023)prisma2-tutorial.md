---
title: (1023)prisma2 tutorial
date: 2019-10-23 12:11:54
category: TIL
---

tutorial 을 따라서 진행해본 내용 정리글이다

## prisma2 설치 후 init

```js
yarn global add prisma2
```

로 prisma2 를 global로 설치한다

```js
prisma2 init hello-prisma2
```

로 prisma2 를 init 한다

다양한 선택사항을 마치고 나면 local에 `hello-prisma2`  
directory 가 생성된다  
(선택사항을 선택하는 내용은 공홈 참조 - https://www.prisma.io/blog/announcing-prisma-2-zq1s745db8i5#getting-started-with-prisma-2)  
(local 에 postgres 에 연결할 계획, typescript 선택)

## prisma2 lift save & up

최초로 prisma2 project 를 시작하면  
user 를 2명 만들어주고 post 를 3개 만들어준다  
이러한 database 의 schema 를

### prisma2 lift save

```js
prisma2 lift save --name 'init'
```

으로 save 해준다

이 과정을 통해 직접 작성한 schema 와  
data CRUD 수정사항을 저장한다
(나중에는 schema 를 수정하는 migration 또한 저장)

이는 schema 를 형상관리 하기 위해서이다  
(따라서 `init` 이라는 commit message 같은걸 추가해야 함)

### prisma2 lift up

위에서 `prisma2 lift save 로 저장한 수정사항을  
실제 DB에 적용하기 위해

```js
prisma2 lift up
```

을 실행한다

## hello-prisma2로 이동 후 yarn, yarn seed, yarn dev

해당 directory로 이동 후 yarn을 실행하여  
package 들을 설치한다

### yarn seed

package.json 에 script 를 보면  
`yarn seed`가 있다  
이는 `"ts-node prisma/seed"`  
즉, prisma 에 seed.ts 를 실행하는 명령어이다

### seed.ts

```js
import { Photon } from '@generated/photon'
const photon = new Photon()

async function main() {
  // user1 , user2 생성한다는 내용~~~
  // create user 어쩌구 저쩌구
}

main().finally(async () => {
  await photon.disconnect()
})
```

기존 seed file 을 보면 user1, user2가 있을 것이다  
user 를 만들어주는 script 내용이다

> 이러한 상태에서 `yarn seed` 를 실행하면 error가 발생한다

왜냐하면 yarn seed 는 위 seed.ts 를 실행하는 것이다  
기존에 이미 user1, user2는 prisma2 init 을 하고  
prisma2 lift save & up 을 통해 databse 에 생성되어 있다  
그런데 yarn seed를 다시 실행하여 user1, user2를 생성하려면  
email이 unique 하다는 database 규칙을 위반하게 된다  
따라서 error가 발생한다

### yarn seed 의 error 해결

```js
const user3 = await photon.users.create({
  data: {
    email: 'david@prisma.io',
    name: 'David',
    password: '$oM44Ig85O3ZFZYZpR3XD7mI8T29eP4znU/.xyJbW', // "secret43"
    posts: {
      create: [
        {
          title: 'Amazing David post',
          content: 'https://google.com',
          published: false,
        },
      ],
    },
  },
})
console.log({ user3 })
```

위에 주석처리된 부분을 user1, user2가 아닌  
위와 같이 user3으로 수정해주면 error가 해결된다  
(email unique한 database 규칙을 지킬수 있기 때문)

### yarn dev

`"dev": "ts-node-dev --no-notify --respawn --transpileOnly src/server",`

yarn dev 를 입력하면 실행되는 script 이다  
src directory 에 server 파일을 실행하고 있다

server file 은 아래와 같다

```js
import { GraphQLServer } from 'graphql-yoga'
import { permissions } from './permissions'
import { schema } from './schema'
import { createContext } from './context'

new GraphQLServer({
  schema,
  context: createContext,
  middlewares: [permissions],
}).start(() =>
  console.log(
    `🚀 Server ready at: http://localhost:4000\n⭐️ See sample queries: http://pris.ly/e/ts/graphql-auth#5-using-the-graphql-api`
  )
)
```

graphQL server 를 실행하는 코드이다  
4000 port 로 접속할 수 있다  
접속하면 graphQL playground 가 나온다  
여기서 이미 작성된 query 를 실행하여  
DB에 data를 CRUD 가능하다

## DB 를 수정하는 3가지 경로

- `src/server` : graphQL playground
- `prisma/seed.ts` : prisma query (ORM)
- `psql` : postgres query

각 경로는 database schema 에 의해 연관되어 있다  
여기에 client 에서 graphQL Playground로  
request 보낼 수 있도록 Apollo 를 사용하면 완성!

최종적으로는 아래 과정을 통해 client 가 DB 를 조작한다

1. front 에서 client 즉, browser 에서  
   graphQL 의 playground 로 API request를 보낸다
2. graphQL 이 prisma 의 photon 으로 query를 보낸다
3. photon 이 query를 DB 조작하는 SQL 로 바꾼다
4. 이 SQL 로 postgres를 조작한다
