---
title: (0703)graphql tutorial
date: 2020-07-03 13:07:46
category: TIL
---

graphql 은 쿼리언어

- shema definition language

## graphql concepts

- Defining schema
  - schema : like api 문서 response type 명시
    - type : schema 의 묶음
      - Root Fields
        - Query : Read data query
        - Mutation : CUD data query
        - Subscription : query for client to subscribe server which data changing
      - 새로운 type 들 : data 의 type 정의
  - resolver : function to fetch data from db by taking arguments for its fiedls
    - 객체 context 를 arguments 로 받아서 적합한 type의 data를 return

## graphql working process

1. server receives a query
2. call the resolver function for the fiedls that are specified in the query's payload
3. that function resolve the query
4. retrieve the data for each field
5.
