---
title: (0715)Promise
date: 2019-07-18 12:07:54
category: TIL
---

Promise는 실행함수를 인자로 받아 promise 객체를 return 하는  
constructor이다. (키워드임. new Promise로 호출)
callback hell을 해결하기 위해 만들어졌다.

```js
var promise1 = new Promise(function(resolve, reject) {
  setTimeout(function() {
    resolve('foo')
  }, 300)
})

promise1.then(function(value) {
  console.log(value)
  // expected output: "foo"
})

console.log(promise1)
// expected output: [object Promise]
```

위 코드에서 new Promise로 promise 객체를 생성했다  
인자로 실행함수를 받고 있다.(객체를 생성하기 전에 실행함수 먼저 실행)  
이 실행함수는 인자로 2가지 resolve, reject라는 함수를 받는다  
실행함수는 보통 비동기 작업을 처리한다  
작업이 완료되면 resolve, 에러가 발생하면 reject를 호출한다

이때 promise의 상태가 바뀐다

##Promise의 상태

![](images/promises.png)

promise는 3가지 상태를 가진다

- pending: 이행하거나 거부되지 않은 초기 상태
- fulfilled: 연산이 성공적으로 완료됨
- rejected: 연산이 실패 함

promise는 처음 호출되면 실행함수가 시작되고  
객체를 return한다 이때 이 객체는 `pending` 상태이다

이때 실행함수는 비동기작업을 처리하는데  
비동기 작업안에서 인자로 받은 resolve, reject 함수를 실행한다

비동기 작업이 문제없이 처리되어 **resolve까지 실행**하면 `fulfilled` 상태가 된다  
이때 객체 promise의 then method를 통해 비동기 실행 결과를 받아 활용할 수 있다

에러가 발생할 경우 **reject 가 실행**되게 비동기 작업 코드를 작성한다  
이때 실제 발생하여 reject가 실행되면 `rejected` 상태가 된다  
객체 promise의 catch method를 통해 비동기 에러 결과를 받아 볼 수 있다

##함수가 promise를 사용하도록 하는 경우

```js
function myAsyncFunction(url) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest()
    xhr.open('GET', url)
    xhr.onload = () => resolve(xhr.responseText)
    xhr.onerror = () => reject(xhr.statusText)
    xhr.send()
  })
}
```

위 처럼 promise 객체를 생성하여 return하는 함수를 선언하면  
사용 가능하다

xhr의 결과물이 성공할 경우 resolve가, 실패할 경우 reject가 실행된다
