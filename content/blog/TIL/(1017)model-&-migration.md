---
title: (1017)prisma tutorial
date: 2019-10-17 14:11:82
category: TIL
---

- data model : data를 요소로 가지는 table 들의 관계를 표현하는 구조나 형식
- data migration : data를 선택, 추출, 준비, 이동하는 process

## prisma 의 장점

1. graphQL 형식의 data model 작성하면, 언어와 DBMS와 상관없이 ORM 클라이언트와 model, data schema 가 자동으로 생성
2. graphQL, REST API의 구현체를 제공해준다
3. 자체 admin page 제공(data crud 쉬움)

## 기존 ORM과 prisma 의 차이

- 기존 ORM(Sequelize) : DB host에 직접 connection 하여 transaction 수행
- prisma : scala로 작성된 prisma의 server가 DB host를 관리

이후, GraphQL 형식의 DataModel 을 정의  
prisma가 언어와 DBMS에 맞게  
실제 DB 배포, client와 model, type definition, GraphQL 구현체까지 제공
