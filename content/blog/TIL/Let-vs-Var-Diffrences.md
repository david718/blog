---
title: Let vs Var Diffrences(with const)
date: 2019-05-17 17:05:30
category: TIL
---

**let**과 **var**는 모두 변수를 선언하는 _키워드_ 이다.  
하지만 둘은 다음과 같은 차이가 있다.

- let : **block scope** 안에서 유효하며, **지역 변수**를 선언한다.
- var : **function scope** 안에서 유효하며, **전역 변수**를 선언할 수 있다.

이 근본적인 차이 때문에 3가지의 차이점이 발생한다.  
먼저 이 3가지 차이점을 살펴본 후,  
변수의 **생성과정**에서 발생하는  
**var**와 **let**, **cosnt**의 차이점까지 추가로 살펴보겠다.  

##let과 var의 차이점

###Global window object
아래에서처럼 let과 var가 모두 global 영역에서 선언될 때  

```js
let letVariable = 'letVariable';
var varVariable = 'varVariable';
```

let은 window object 즉, 전역 객체를 통해 접근할 수 없지만
var는 접근이 가능하다.(전역 변수 선언이 가능)  

```js
console.log(window.letVariable); //undefined
console.log(window.varVariable); //varVariable
```

###Block scope
Javascript에서 Block scope는 3가지 경우에 사용한다.

1. for 문
2. while 문
3. if 문

let은 이 Block scope 안에서만 유효한 **지역 변수**를 선언한다.

for문을 예로 들어 확인해보자.

```js{4}
for(let i = 0; i < 10; i++) {
  console.log(i);
} //0,1,2,,, 9
console.log(i); //throw error `i is not defined`
```

위와 같이 for문 안에서 let으로 선언한 i는  
해당 for문이 가지는 **Block scope** 안에서 **유효한 지역 변수**이다.  
  
하이라이팅 된 console은 for문의 Block scope 밖이므로  
let으로 선언한 i 가 _유효하지 않다_.  
즉 선언되지 않았다는 에러가 발생한다.

반면에 var의 경우

```js{4}
for(var i = 0; i < 10; i++) {
  console.log(i);
} //0,1,2,,, 9
console.log(i); //10
```

똑같이 for문 안에서 선언했지만
for문 밖에서도 하이라이팅된 console을 통해 접근이 가능하다.

###Redeclaration
let은 재선언 할 수 없지만  
var는 재선언 할 수 있다.

재선언? 그게 뭐지?
선언을 다시한다는 말인데, 선언은 변수의 생성 과정 중 첫번째 단계이다.

##변수의 생성 과정에서 발생하는 차이점
###변수의 생성 과정
변수는 3가지 단계를 거쳐서 생성된다.

1. 선언 : 실행컨텍스트의 변수객체에 변수를 등록한다.
2. 초기화 : 변수객체에 등록된 변수에 초기 값을 할당한다.
3. 할당 : 초기화가 끝난 변수에 새로운 값을 할당한다.

###var의 변수 생성 과정
var는 선언과 동시에 초기화가 이루어진다.

1. 선언 & 초기화 : 실행컨텍스트의 변수객체에 변수가 등록되고  
따로 초기 값을 할당하지 않으면  
**undefiend**가 초기 값으로 할당된다.(_재선언 가능_)
2. 할당 : 이후 자유롭게 할당이 가능하다.

###let의 변수 생성 과정
let은 선언과 초기화가 따로 이루어진다.

1. 선언 : 실행컨텍스트의 변수객체에 변수를 등록한다.(_재선언 불가능_)
2. 초기화 : 변수에 초기 값을 할당해야 한다.(값이 없는 상태)  
(따라서 호이스팅은 되지만 _reference error 발생_)
3. 할당 : 초기화 이후 다른 값을 할당할 수 **있다.**

###const의 변수 생성 과정
const는 선언 후, 값을 변경할 수 없다.(상수처럼 사용)

1. 선언 & 초기화 : 실행컨텍스트의 변수객체에 변수가 등록되고  
반드시 초기 값을 할당해야 한다.(_안할 경우 초기화 error 발생_)
2. 할당 : 초기화 이후 다른 값을 할당할 수 **없다.**

##결론
**변수의 생성과정**과 **유효범위**에서 var와 let, const는 차이가 있으므로  
규칙을 파악한 후 주의해서 사용해야 불필요한 error를 발생시키지 않을 수 있다.

##References
- https://medium.com/sjk5766/var-let-const-%ED%8A%B9%EC%A7%95-%EB%B0%8F-scope-335a078cec04
- https://codeburst.io/difference-between-let-and-var-in-javascript-537410b2d707
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/let
