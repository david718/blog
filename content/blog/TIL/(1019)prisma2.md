---
title: (1019)prisma2
date: 2019-11-04 17:11:38
category: TIL
---

prisma2 구성 요소는 아래 2가지이다

- `Photon` : auto-generated ob the prisma datamodel, type-safe database client
- `Lift` : Declarative data modeling and database migrations

## photon

photon 은 prisma datamodel(schema) 에 의해 자동 생성된다  
type-safe database client 이다

client에서 바로 database로 query를 보낼 수 있도록 돕는  
library이다

### @generate

```js
import { Photon } from 'node_modules/@generate'
const photon = new Photon()
```

위와 같이 photon을 가져와서 instance 를 사용한다

## lift

prisma의 declarative data model definition 에 기반한다  
따라서 data model 을 수정한 후에 lift CLI 인 `apply`만 쓰면  
수정사항이 database에 직접 반영 된다

모든 migration 은 기록되므로 log 를 따라 수정 가능하다

## photon 과 lift 의 연관성: datamodel

photon과 lift는 각각 data model definition 과 연관이 있다

- photon : CRUD API 를 model 들에게 제공한다
- lift : database의 schema 를 나타낸다
