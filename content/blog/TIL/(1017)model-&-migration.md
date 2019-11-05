---
title: (1017)prisma tutorial
date: 2019-10-17 14:11:82
category: TIL
---

- data model : data를 요소로 가지는 table 들의 관계를 표현하는 구조나 형식
- data CRUD : data 자체를 create, read, update, delete 하는 것
- database migration : model 들의 관계인 schema 에 대한 모든 수정

## prisma

prisma는 전통적인 orm을 대체하고  
database workflow를 간단하게 만든다

- Access : 자동으로 만들어진 prisma client로 Type-safe database에 접근한다
- Migrate : 선언적 data modelling 과 migrations
- Manage : prisma admin으로 visual 하게 database 를 관리한다

### prisma 의 장점

1. custom되어 자동 생성된 prisma client로 Type-safe database 에 접근 가능
2. RDB와 transactions 가 가능한 간단한 APIs
3. 자체 admin page 제공(visual 이어서 관리 쉬움)
4. graphQL 형식의 data model 작성하면, 언어와 DBMS와 상관없이 ORM 클라이언트와 model, data schema 가 자동으로 생성
5. graphQL, REST API의 구현체를 제공해준다

### 기존 ORM과 prisma 의 차이

- 기존 ORM(Sequelize) : DB host에 직접 connection 하여 transaction 수행
- prisma : scala로 작성된 prisma의 server가 DB host를 관리

이후, GraphQL 형식의 DataModel 을 정의  
prisma가 언어와 DBMS에 맞게  
실제 DB 배포, client와 model, type definition, GraphQL 구현체까지 제공

## Datamodel, Prisma client, Prisma server

- Datamodel : application의 model 들을 정의한다, prisma client API의 기초이다
- prisma server : 독립적인 infrastructure 로 database를 관장한다
- prisma client : prisma server 와 연결되어 data를 crud 할 수 있도록 돕는 자동으로 생성되는 library이다

### Datamodel

Datamodel을 통해 database 의 model의 schema 를 정의한다

prisma client 의 API를 운영할 기초를 serve 한다  
graphQL schema language 를 사용한다  
따라서 객체 type 과 filed 로 구성되어 있다

### prisma client

prisma 문법으로 작성한다  
API server 안에 있는 전통적인 ORM을 대체한다  
database를 관장하는 prisma server 에 request 를 보내서  
data를 CRUD 한다 (JOIN 도 쉽게 한다)  
(Native GraphQL 도 사용 가능하다 `$graphql`)

### prisma server

prisma client 가 보낸 request를 database query 로 바꿔준다  
(여기서 직접 database 와 연결되어 data 를 CRUD 함)

database와 연결된 독립적인 infrastructure component이다  
(따라서 AWS, GCF, Azure 등 어디에서나 deploy 가능)  
deploy 될 때 configuration이 설정되어야 한다  
(user credentials 와 database connection details 포함하여)

## Prisma under the Hood

prisma 는 graphQL 과 연관성이 크다

### prisma services

prisma service는 Datamodel 에게  
graphQL CRUD 를 제공한다  
이 service가 prisma server 에서 실행되어  
prisma client가 보낸 request를 database query로 바꿔준다

`prisma.yml` 과 `schema.prisma` 에 의해 config 된다
