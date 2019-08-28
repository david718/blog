---
title: (0821)graphQL basic concepts
date: 2019-08-21 18:08:22
category: TIL
---

graphQL은 over fetching 혹은 under fetching을 막고  
필요한 data만 가져올 수 있도록 도와주는 server 형식이다

같은 카테고리로는 RESTful API 가 있다

##graphQLServer

```js{3}
import { GraphQLServer } from 'graphql-yoga'

const server = new GraphQLServer({})

server.start(() => {
  console.log('graphql server running')
})
```

첫 graphQLServer 는 위처럼 만들 수 있지만 error다

가장 먼저 GraphQLServer 안에 object에  
2가지 인자가 필요하다

##typeDefs
graphQL은 query를 사용해서 data를 주고 받는다  
이 query의 type을 description 해줘야한다

확장자를 graphql로 하여 schema라는 파일을 만든 후  
그곳에 query를 description 해준다

```js
type Query {
  name: String!
}
```

이후에 아래처럼 GraphQLServer에 첫번째 인자를 넣는다

```js
const server = new GraphQLServer({
  typeDefs: 'graphql/schema.graphql',
})
```

여전히 에러다

##resolvers
graphQL은 단순히 형식이기때문에  
javscript가 이해하기 위해서는 resolver가 필요하다

resolver는 javascript 함수로  
graphQL의 query가 무엇을 return해야 하는지 설명해준다

```js
const resolvers = {
  Query: {
    name: () => 'david',
  },
}

export default resolvers
```

'david'라는 값을 반환하는 함수를 name에 할당한다

이후 아래처럼 server를 생성해준다

```js
const server = new GraphQLServer({
  typeDefs: 'graphql/schema.graphql',
  resolvers,
})
```

server에 typeDefs 로 `schema.graphql`과  
resolvers를 할당하면 정상 작동하는 것을 확인할 수 있다
