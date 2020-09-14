---
title: (0703)graphql tutorial
date: 2020-07-03 13:07:46
category: TIL
---

graphql 은 API 를 정의하는 쿼리언어

- shema definition language

## graphql concepts

- Defining schema
  - schema : like api 문서 response 명시된 type 의 모음
    - type : field 와 해당 field 의 type 그리고 value 로 구성된다
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
  - query : client 가 필요한 data 의 형태를 server 에게 요청하기 위해 작성하는 code 이를 처리하기 위한 resolver 가 필요하다
    - root field : 위의 schema 에서 Root Field 의 Query 에 해당하는 값 중 하나로 요청하는 query 의 이름이라고 생각할 수 있다
      - payload(fields) : root field 에 해당하는 query 를 통해 client 가 받고 싶은 data 값들의 field 값들, field 이름을 나열한다
  - resolver : type 안에 field 와 1:1 대응으로 연결된 function. 연결된 field 에 해당하는 data value 를 return 한다. query와 1:1 대응이기도 하다
    - arguments
      - context : db 와 연결할 때 사용. redux context 와 비슷

## graphql working process between server and client

1. client write query for fetching the data
2. server receives that query
3. server call the resolver function that related with fields that are specified in the query's payload
4. when the resolver function invoke, return data values for the fields
5. server response the data values for client's query
6. client render the data through components

## graphql getting started

1. define data set
2. define GraphQL schema
   - type
     - root fileds
       - Query
       - Mutation
       - Subscription
     - other types
3. define a resolver
