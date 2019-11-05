---
title: (1019)prisma2
date: 2019-10-19 17:11:38
category: TIL
---

prisma2는 prisma 보다 더욱 간단한 workflow를 가진다

## prisma2의 구성요소

prisma2 구성 요소는 아래 2가지이다

- `Photon` : auto-generated ob the prisma datamodel, type-safe database client
- `Lift` : Declarative data modeling and database migrations

### photon

photon 은 prisma datamodel(schema) 에 의해 자동 생성된다  
type-safe database client 이다

client에서 바로 database로 query를 보낼 수 있도록 돕는  
library이다

#### @generate

```js
import { Photon } from 'node_modules/@generate'
const photon = new Photon()
```

위와 같이 photon을 가져와서 instance 를 사용한다

### lift

prisma의 declarative data model definition 에 기반한다  
따라서 data model 을 수정한 후에 lift CLI 인 `apply`만 쓰면  
수정사항이 database에 직접 반영 된다

모든 migration 은 기록되므로 log 를 따라 수정 가능하다

### photon 과 lift 의 연관성: datamodel

photon과 lift는 각각 data model definition 과 연관이 있다

- photon : CRUD API 를 model 들에게 제공한다
- lift : database의 schema 를 나타낸다

## prisma2와 기존 ORMs 의 차이점

prisma는 database workflow를 논리적으로 구성된  
하나의 생태계 안에서 통합하여 다루기때문에 개발 경험을 개선한다

### declarative data model

기존의 ORM은 method 로 model 을 생성했다  
하지만 prisma는 GraphQL schema language 를 사용하여  
declarative 즉 선언적으로 data model 을 생성한다  
동시에, 이 data model 은 Photon 에게 data access API를 제공한다

모든 workflow는 이 schema 즉, data model 을 기반으로  
형성되기 때문에 팀원 전체가 변경사항을 인지할 수 있고 적응할 수 있다

### photon is a type-safe and auto-generated database client

전통적인 ORM 은 database 에 mapping layer 가 필요했다  
직접 data model 을 수정하여 define 하고 import 해야 했다  
photon은 자동으로 data access API를 만든다  
게다가 type-safe 이다

### safe & resilient migrations with Lift

전통적인 database schemas 를 migration 하는 작업은 어렵다  
하지만 lift 를 사용하면 아주 간단해진다

1. declarative data model definition 을 수정한다
2. 수정 사항을 저장한다
   ```js
   prisma2 lift save // stores a new migration folder on the file system
   ```
3. 저장한 수정 사항을 database 에 반영한다
   ```js
   prisma2 lift up // applies the migration from the previous step
   ```

마치 git 을 활용한 code 형상관리처럼 lift는  
database schema(data model definition)을 관리해준다

## prisma2 의 개선점(prisma1 에 비해)

prisma1 에서는 아래 2파일이 존재했다

- prisma.yml : root configuration file for the project
- datamodel.prisma: Abstracts the database and provides foundation for generated prisma client API

하지만 prisma2 에서는 `schema.prisma` 로 merge 되었다

### Improved data access API & type-safe field selection in Photon

photon 은 prisma client API 에 비해 조금 더 발전된 data access API 를 제공한다

```js
const newUser = await photon.users.create({
  data: {
    name: 'Alice',
  },
})

const allUsers = await photon.users.findMany()
const oneUser = await photon.users.findOne({
  where: { id: 1 },
})
```

더 mongoose 와 같이 method 가 분리되고 통합되었다

### prisma server optional

prisma1에서는 Query Engine과 Migration Engine 이  
prisma server 에서 돌아갔다(prisma server 필수)  
하지만 prisma2 에서는 아래 2가지가 분리되어 각각으로 흡수되었다

- Query Engine : photon 안에서(photon은 API server 에서 실행 가능)
- Migration Engine : CLI 에서(Lift 가 ME 이므로 CLI에서 직접 DB 조작)

따라서 prisma server 가 optional 하게 바뀌었다
