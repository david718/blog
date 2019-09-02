---
title: Differences between Asynchronous and Non-blocking
date: 2019-09-01 10:09:48
category: Developments
---

일반적으로 Asynchronous는 Non-blocking과 비슷한 개념으로 이해한다
Asynchronous 방식으로 function을 호출하고 실행하는 서버는  
Non-blocking 방식으로 동작하는 것처럼 보이기 때문이다

하지만 Asynchronous와 Non-blocking은 기준이 다르다

##Blocking or Non-blocking의 관심사
Blocking은 호출된 function이 바로 return 하느냐 마느냐가 관심사이다

호출된 function이 바로 return해서 호출한 function에게 제어권을 넘겨주고  
호출한 function이 다른 일을 할 수 있도록 한다면 Non-blocking이다

그렇지 않고 호출된 function이 자신의 모든 작업을 마칠 때까지  
호출한 function에게 제어권을 넘겨주지 않고 대기하게 만든다면 blocking이다

##Synchronous or Asynchronous의 관심사
Synchronous는 호출된 function의 작업 완료 여부를 누가 신경쓰냐가 관심사이다

호출된 function에게 callback을 전달해서,  
호출된 function의 작업이 완료되면 전달받은 callback을 실행하고  
호출하는 function은 작업 완료 여부를 신경쓰지 않으면 Asynchronous이다

호출하는 fucntion이 function의 작업 완료 후 return을 기다리거나,  
호출된 function으로부터 바로 return 받더라도  
작업 완료 여부를 호출하는 function 스스로 계속 확인하며 신경쓰면 Synchronous이다

##비슷한 동작, 다른 관심사
그럼 Asynchronous와 Non-blocking  
또는 Synchronous와 blocking 은 무엇이 비슷한가?  
동작이 비슷하다

Blocking과 Sync는 막거나 기다리거나 하는 등의 동작을 한다  
반면, Non-blocking과 Async는 막지도 않고 완료되면 알아서 처리하는 동작을 한다

하지만 다른 관심사를 가진다  
Non-Blocking은 호출된 function이 바로 return하느냐를 확인하는 반면,  
Async는 작업 완료 여부를 누가 신경쓰고 있느냐를 확인한다

구체적인 예시를 보자

##Blocking-Async: Node.js & MySQL
Node.js에서 callback hell을 실행하며 Async로 동작하는 과정에서  
DB 작업을 실행하기 위해 MySQL에서 제공하는 드라이버를 호출하게 됨
이 드라이버가 Blocking 방식

Blocking-Async는 NonBlocking-Async 방식을 쓰는데 그 과정에서  
하나라도 Blocking으로 동작하는 function(MySQL driver 등)을 포함하면
그게 바로 Blocking-Async이다

##Non-blocking-Sync: isDone()
isDone이라는 method는 호출하는 fucntion이 완료 여부를 계속 신경쓴다  
동시에 return을 바로하는 method 이므로  
Non-blocking이자 Sync라고 볼 수 있다

사실 효율적인 자원 활용 측면에서 본다면 좋은 동작과정은 아니다

> 성능과 자원의 효율적 활용 측면에서 가장 유리한 모델은 Async-NonBlocking 모델이다

##다른 구분 방법

- Blocking/Non-blocking은 호출한 function의 입장 차이
- Sync/Async는 callback 처리 방식의 차이

##references

- https://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/
- http://www.programmr.com/blogs/difference-between-asynchronous-and-non-blocking
