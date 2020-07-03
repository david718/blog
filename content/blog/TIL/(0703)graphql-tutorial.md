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
    - type
      - Root Fields
        - Query : Read data query
        - Mutation : CUD data query
        - Subscription : query for client to subscribe server which data changing
      - Scalar
        - Int
        - Float
        - String
        - Boolean
        - ID
      - Object
  - resolver : function to fetch data from db by taking arguments for its fiedls : query(객체, context..) 를 arguments 로 받아서 해당하는 database 로 가서 적합한 type의 data를 return 하는 function
    - arguments
      - context : db 와 연결할 때 사용. redux context 와 비슷

## graphql working process

1. server receives a query
2. call the resolver function for the fiedls that are specified in the query's payload
3. that function resolve the query
4. retrieve the data for each fields

## graphql get started

1. define data set
2. define GraphQL schema
   - fileds
     - root fileds
       - Query
       - Mutation
       - Subscription
     - other fileds
3. define a resolver
