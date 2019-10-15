---
title: (0925)generator(js)
date: 2019-09-25 13:10:54
category: TIL
---

generator는 iterator의 한계를 극복하기 위해 만들어진  
function의 일종이다

이를 위해서는 iterator의 개념과 그 한계를 알아야 한다

## iterator

반복기는 javascript의 반복문의 내부 동작에 접근할 수 있는  
api라고 생각하면 쉽다(Symbol.iterator로 접근 가능)

```js
let factorial = {
  [Symbol.iterator]() {
    let count = 1
    let cur = 1
    return {
      next() {
        ;[count, cur] = [count + 1, cur * count]
        return { done: false, value: cur }
      },
    }
  },
}
for (let n of factorial) {
  if (n > 10000000) {
    break
  }
  console.log(n)
} // 1, 2, 6, 24, 120, 720, 5040, 40320, ...
```

factorial 에 Symbol.iterator로 선언한 iterator를 할당했다  
이는 next()를 return 하고 있고  
next()는 객체를 return 하고 있다

for ... of 반복문으로 fatorial의 n 에 접근할 수 있다
여기서 n 은 cur이다(즉 현재값 cur)

위와 같은 반복기가 등장하기 전에는 어떻게 반복기를 만들었을까?

## iterator 이전에 반복기

```js
var oldFactorial = function() {
  var count = 1
  var cur = 1
  return {
    next: function() {
      cur *= count++
      return { done: false, value: cur }
    },
  }
}
var fact = oldFactorial()
fact.next().value // 1
fact.next().value // 2
fact.next().value // 6
```

이 function을 보면 이해가 쉽다
oldFactorial은 객체를 return해주는 function이다  
이 객체는 next 라는 key를 가지고 있다  
그 값은 또다른 function이다  
그 function은 또다른 객체  
`{ done: false, value: cur }`를 return한다

즉 정리하면 fact라는 변수에 oldFactorial을 호출한 결과값  
즉 객체를 return해주고 있다 이 객체는 next를 호출할 수 있다  
next를 호출하면 결과값으로 value를 갖게된다  
value가 반복문이 실행되는 현재 값이다

> 이방법은 자동으로 반복문이 실행될 수 없는 한계가 있다

따라서 iterator가 등장한 것이다

## iterator의 한계

그러나 iterator또한 한계가 있다  
자동으로 실행은 되지만 수동으로 실행이 불가능하다는 점이다  
iterator의 수동 실행이 불가능한 한계를 극복하기 위해  
generator가 등장했다

## generator

generator는 문법적인 요소가 추가되었다  
`*` 과 `yield` 가 추가된 요소들이다

```js
function* generator() {
  let cur = 1
  let count = 1
  while (true) {
    ;[count, cur] = [count + 1, cur * count]
    yield cur
  }
}
let gen = generator()
gen.next().value // 1
gen.next().value // 2
gen.next().value // 6
```

위 generator를 보면 return 값이 따로 없다  
대신 yield가 있다
여기서 yield는 next 가 return하는 객체의 value 값을 return 한다

### `yield`

`yield`를 만나면 그 뒤에 오는 값을  
next() 객체의 value 값으로 할당한다

```js
function* generator() {
  yield 1
  yield 'david'
  yield ['hwang', 'david']
}
let gen = generator()
gen.next().value // 1
gen.next().value // 'david'
gen.next().value // ['hwang', 'david']
gen.next() // { value: undefined, done: true }
```

3개의 yield를 통해서 next 가 return하는 객체를 할당했다  
이 후 4번째로 next 를 호출하면  
value는 undefined고 done은 true인 객체를 return 한다

### `yield*`

```js
function* stringCutter(string) {
  yield* string
}
const cutter = stringCutter('david')
cutter.next().value // 'd'
cutter.next().value // 'a'
```

yield의 해당하는 value를 자동으로 쪼개 반복한다  
(Symbol.iterator 가능한 값만 동작)

### next 를 통해 `yield`에게 값 전달하기

```js
function* deliverGenerator() {
  console.log('generator called')
  let val = yield
  console.log('1st', val)
  val = yield 2
  console.log('2nd', val)
  val = yield
  console.log('3rd', val)
}
let delGen = deliverGenerator()
delGen.next(1) // 'generator called'
delGen.next(2) // '1st 2', next의 value는 2
delGen.next(3) // '2nd 3'
delGen.next(4) // '3rd 4', next의 done은 true
```

next()의 parameter로 넣은 값은  
generator 안에 yield에게 전달된다  
그것과 별개로 yield 뒤에 값이 next().value 값이된다

### return 과 throw 호출 가능

`delGen.return()` 하면 generator는 멈춘다
next() 의 done 은 true 가 된다  
`delGen.throw()`는 return 과 같이 generator를 멈추고  
추가로 error 를 발생한다(try catch 에서 catch 로 내보내기 가능)
