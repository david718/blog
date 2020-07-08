---
title: (0708)redux concepts
date: 2020-07-08 14:07:35
category: TIL
---

## redux 등장 배경 (react 만 쓸 때 한계점)

- 가장 위 component 가 가장 아래 component 에게  
  props 를 전달하기 위해 드릴링이 발생한다  
  (child component 에게 props 전달,  
  그 아래 child component 에게 props 전달,  
  그 아래 child component 에게 props 전달.. 반복)
- 아래 component 에서 위에 component 의 state 를 바꾸기 힘들다

## redux란?

react component 의 state 관리 library이다  
state와 reducer로 store를 만들어 state를 관리한다

## redux 의 동작 과정

redux 는 state 와 reducer 로 store 를 만든다  
store 는 reducer를 통해 state 를 관리한다  
action 이 dispatch에 의해 reducer로 전달되면  
reducer 가 state 를 바꾼다

각 component 에서는 dispatch를 통해  
store 로 action 을 보낼 수 있다  
그 결과 store 안에 state 값이 바뀐다  
또한 store 안에 state 에서 값을 직접 읽어올 수 있다

## redux 의 한계?

state 를 저장하는 위치가 browser의 memory 이므로  
새로운 http 통신을 하면 state 가 사라진다

## 한계 극복 방안? redux-persist

redux-persist 라는 library 가  
redux의 state를 localStorage에 저장하여  
새로운 http 통신을 하더라도 state 를 불러올 수 있게 돕는다

## redux 의 또다른 한계?

비동기 redux action 에 대해서 store 에 반영이 어렵다

## 또다른 한계 극복 방안? redux-saga

redux-saga 라는 library 가  
saga-effect 를 통해 async action을 관리하여  
flux 구조를 유지하며 state 관리를 가능하게 한다

## Nextjs(SSR) 에서의 redux 활용

SSR의 구현체인 nextjs 에서 redux 를 활용하기 위해서는  
server 와 client 에서 각각 store를 관리하여야 한다  
총 2개의 store 가 관리되는데  
이때, 하나의 store 가 변화되면  
똑같은 변화가 나머지 store에도 발생하여 같은 state 값을 유지해야 한다

## how to hydrate state between client and server state in nextjs

next-redux-wrapper 라는 library 는  
hydrate 이라는 특별한 action 으로 이를 해결한다

server 에서 따로 관리하는 store 가  
client 에서 http 통신을 요청할 때마다  
hydrate 이라는 특별한 action 을 발생한다

이 hydrate action 은 payload 로  
현재 server의 state 값을 가지고 있다

따라서 client 의 state 를 server 의 state 로 덮어쓴다  
그럼 client 와 server 의 state 값이 일치된다
