---
title: (0713)javascript 메모리 관리
date: 2019-07-17 10:07:70
category: TIL
---

javascript 에서는  
무언가(객체, 문자열 등)가 생겨날 때 메모리가 할당(allocate)되며  
더 이상 사용하지 않을 때 할당했던 메모리를 다시 회수(release)한다  
  
이때 자동으로 메모리가 회수되는 과정을 garbage collection이라 한다  
하지만 버그 또는 환경의 제약으로 인해  
garbage collection이 불가능한 경우가 발생하므로  
메모리 관리에 대해 기본적인 지식이 필요하다  

##메모리 생명주기
아래 3단계를 거쳐 메모리는 태어나고 죽는다

1. **allocate:** 메모리 할당. 프로그램이 사용할 수 있도록 운영체제가 메모리를 할당.
2. **use:** 메모리 사용. 할당된 메모리를 프로그램이 사용하는 단계. 개발자가 코드상에서 할당된 변수를 사용함으로써 읽기와 쓰기 작업이 이루어짐  
3. **release:** 프로그램에서 필요하지 않은 메모리 전체를 되돌려주어 다시 사용 가능하게 만드는 단계.

###하드웨어 수준에서의 메모리의 정의
컴퓨터 메모리는 많은 수의 flip flop이 나타내는 비트로 구성되어 있다  

####flip flop과 비트의 정의
상태 정보를 저장하기 위해 2가지 상태를 가지는 circuit이다  
컴퓨터에서 flip flop은 circuit의 한 종류인 트랜지스터를 몇개 가지고 있다  
이 몇개의 트랜지스터를 통해 flip flop에 하나의 비트를 저장할 수 있다  
  
비트는 이진수를 뜻하는 ‘Binary Digit’의 약자로,  
컴퓨터에서 CPU가 처리하는 데이터의 최소 단위 크기를 의미한다  
  
개별 flip flop은 고유한 식별자를 통해 위치를 확인할 수 있으므로  
flip flop을 읽거나 쓰는 것이 가능해진다  
  
컴퓨터 메모리는 이러한 개별 filp flop에 쓰여있는  
비트로 구성된 커다란 하나의 배열과 같다  
  
####바이트
비트, 즉 이진수가 인간에게 익숙하지 않기에  
비트를 더 큰 그룹으로 모아 숫자로 표현하기로 약속한 것이 바이트이다  
(8비트는 1바이트이다)

##일반적인 언어에서 메모리 관리 과정
먼저 메모리에 저장되는 것들은 아래 2가지이다

- 프로그램에서 사용되는 변수와 기타 데이터
- 운영체제 및 개별 프로그램의 코드

운영체제와 컴파일러가 위에 것들을 메모리에 저장하고 삭제하는 과정  
즉, **메모리 관리 과정**은 어떻게 이루어질까?
이는 **정적할당** or **동적할당**에 따라서 달라진다

###정적할당 메모리 관리 과정

1. 컴파일러가 코드를 컴파일한다
2. 컴파일러는 원시 데이터 타입을 검사해서 메모리 필요량을 미리 계산한다
3. 메모리 필요량이 stack space라는 곳에서 프로그램에 할당된다  
(stack space에서는 함수가 호출되면 해당 함수의 메모리가 기존 메모리 위에 stack처럼 쌓인다)
4. 함수가 종료되면 stack처럼 마지막에 할당된 메모리부터 회수된다

###동적할당 메모리 관리 과정
특정 변수의 메모리 필요량을 미리 계산할 수 없다면?  
즉 사용자가 제공하는 값에 따라 메모리 필요량이 달라진다면  
컴파일러는 stack space에 메모리 필요량을 미리 할당할 수 없다  
  
따라서 운영체제에게 적당량의 메모리를 요구해야 한다  
이러한 메모리는 heap space에서 할당 받는다  

###메모리의 정적할당과 동적할당의 차이

- 정적할당
  - compile time에 메모리 필요량 알아야 함
  - compile time에 수행됨
  - stack space에서 할당 됨
  - FILO(first in, last out)

이에 비해 동적할당은 아래의 특징을 가진다

- 동적할당
  - compile time에 메모리 필요량 모를 수 있음
  - run time에 수행됨
  - heap space에서 할당 됨
  - 특정 할당 순서가 없음

동적 할당은 포인터에 대해 이해해야 한다  
(나중에 추가로 포인터에 대해 깊이 공부해보자)  
  
##자바스크립트에서의 메모리 관리
자바스크립트는 기본적으로 동적할당이다  
이러한 특성때문에 일반적인 언어와 다른 메모리 관리 과정을 가진다

###자바스크립트에서의 메모리 할당
변수 할당 시점에 메모리 할당을 자동으로 수행한다  

```js
var n = 374; // 숫자에 대한 메모리 할당
var s = 'sessionstack'; // 문자에 대한 메모리 할당
var o = {
  a: 1,
  b: null
}; // 객체 및 그 값에 대한 메모리 할당
var a = [1, null, 'str'];  // (객체와 같음) 배열과 그 값에 대한
                           // 메모리 할당
function f(a) {
  return a + 3;
} // 함수에 대한 할당(함수는 호출할 수 있는 객체임)
// 함수 표현식 또한 객체를 할당함
someElement.addEventListener('click', function() {
  someElement.style.backgroundColor = 'blue';
}, false);
```

아래와 같은 몇몇 함수 호출은 객체 할당과 같은 결과를 가져온다  

```js
var d = new Date(); // Date객체 할당
var e = document.createElement('div'); // DOM 요소 할당
```

메소드는 새로운 값이나 객체를 할당할 수 있다  

```js
var s1 = 'sessionstack';
var s2 = s1.substr(0, 3); // s2는 새로운 문자열이 됨
// 문자열은 불변(immutable)이므로 자바스크립트는 메모리를 할당하지 않고
// [0, 3]의 범위만 저장할 수도 있음
var a1 = ['str1', 'str2'];
var a2 = ['str3', 'str4'];
var a3 = a1.concat(a2);
// 4개의 요소를 가진 새로운 배열은
// a1과 a2 요소의 연결이 됨
```

###자바스크립트에서 메모리의 사용
자바스크립트에서 할당된 메모리를 사용하는 것은  
그 메모리 내에서 읽거나 쓰는 것을 뜻한다  
아래 3가지 경우에 메모리를 사용한다고 할 수 있다
  
- 객체의 속성을 읽거나 쓸 때  
- 변수의 값을 읽거나 쓸 때  
- 함수에 인수를 넘겨줄 때

###할당된 메모리를 더이상 사용하지 않을 때 회수하기
자바스크립트에서는 garbage collector라는 소프트웨어가 내장되어 있음  
메모리 할당을 추적하여 할당된 메모리가 더이상 사용되지 않는 시점을 파악한 후  
자동으로 회수하는 기능을 담당  
  
이러한 과정을 추정에 기반. 알고리즘으로 시점을 파악할 수 없기 때문.  
  
그렇다면 기준은?  
  
> 어떤 메모리를 가리키는 모든 변수가 모든 스코프를 벗어났을 때  
  
즉 더이상 접근 불가능한 메모리를 수집  
하지만 이는 보수적인 기준이다  
왜냐면 스코프 안에 있는 변수 중 이후 다시는 접근하지 않는 경우 있기에  

###가비지 컬렉터(garbage collector) 알고리즘과 한계 극복
가비지 컬렉터가 위 한계를 극복하기 위한 노력에 대해서는 생략한다
(다음 링크 참조: https://engineering.huiseoul.com/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%9E%91%EB%8F%99%ED%95%98%EB%8A%94%EA%B0%80-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B4%80%EB%A6%AC-4%EA%B0%80%EC%A7%80-%ED%9D%94%ED%95%9C-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EB%88%84%EC%88%98-%EB%8C%80%EC%B2%98%EB%B2%95-5b0d217d788d)

##메모리 누수
이전에는 프로그램에서 사용했다가 더이상 필요하지 않게 된  
메모리 중에서 아직 회수되지 않은 메모리  
  
오직 개발자만이 어떠한 메모리를 회수해야 하는지 알 수 있음  

##자바스크립트에서 메모리 누수 예시
자바스크립트에서 메모리 누수가 발생하는 경우는 주로 아래 4가지이다

###1. 전역 변수
자바스크립트는 선언되지 않은 변수가 참조되는 경우  
전역 객체에 새로운 변수를 생성한다  
(브라우저 상이라면 전역 객체는 window가 된다)

```js
function foo(arg) {
    bar = "some text";
}
```

위 코드는 아래와 같다

```js
function foo(arg) {
    window.bar = "some text";
}
```

또한 this를 사용할때도 위에서처럼  
전역 객체에 새로운 변수를 생성하는 실수가 발생할 수 있다  

```js
function foo() {
    this.var1 = "potential accidental global";
}
// 다른 함수 내에 있지 않은 foo를 호출하면 this는 글로벌 객체(window)를 가리킴
foo();
```

> 자바스크립트에서는 파일 상단에 use strict을 사용하면 위의 모든 실수를 방지할 수 있다

전역 변수는 메모리 누수의 주원인이기에 주의해서 사용하며  
사용이 끝나면 null을 할당하거나 다른 변수를 할당해서  
메모리 누수를 방지하는 것이 좋다  

###2. 잊혀진 타이머 혹은 콜백함수
대부분 최신 브라우저들은 타이머 혹은 콜백에서 참조하는 변수들이  
더이상 사용되지 않을 경우 해당 변수의 메모리를 회수한다  
  
하지만 그렇지 않을 경우도 있기에 수동으로 해주는 방법을 알면 좋다  
아래 코드가 수동으로 메모리를 회수하는 방법의 예시이다  

```js
var element = document.getElementById('launch-button');
var counter = 0;
function onClick(event) {
   counter++;
   element.innerHtml = 'text ' + counter;
}
element.addEventListener('click', onClick);
// 필요한 작업 수행
element.removeEventListener('click', onClick);
element.parentNode.removeChild(element);
// 이제 요소들이 스코프를 벗어나게 되면
// 순환참조를 잘 처리하지 못 하는 구형 웹브라우저에서도
// 해당 요소와 onClick 콜백을 가비지컬렉터가 가져감
```

element에 DOM node가 할당되어 있다
onClick이라는 callback이 event listener로 등록되었다  
  
필요한 작업을 수행한 후  
removeEventListener()를 호출하여 등록된 event listener를 제거했다  
또한 removeChild()를 호출하여 DOM node 할당을 제거했다  

그 결과 element, onClick은 scope에 속하지 않게 되었다  
따라서 garbage collector가 이 두 변수들의 메모리를 회수하게 된다  

###3. 클로져
클로저란 함수가 리턴하는 함수이다.  
리턴된 함수는 리턴한 함수의 변수에 접근할 수 있다  
  
아래는 클로져가 참조하는 스코프를 공유하고 있는  
다른 클로저를 통해 메모리 누수가 발생하는 경우이다

```js
var theThing = null;
var replaceThing = function () {
  var originalThing = theThing;
  var unused = function () {
    if (originalThing) // 'originalThing'에 대한 참조
      console.log("hi");
  };
  theThing = {
    longStr: new Array(1000000).join('*'),
    someMethod: function () {
      console.log("message");
    }
  };
};
setInterval(replaceThing, 1000);
```

unused와 someMethod가 같은 scope를 공유하고 있다  
theThing이라는 변수가 매개체가 되었다  
  
unused가 계속 originalThing을 참조하고 있으므로  
theThing의 someMethod는 활성화된 상태가 강제로 유지된다  

(자세한 링크는 아래참조)
https://blog.meteor.com/an-interesting-kind-of-javascript-memory-leak-8b47d2e7f156

###4. DOM에서 벗어난 요소 참조
DOM노드를 data structure에 저장하는 경우  
만약 해당 DOM노드를 제거하고자 할 때에는  
DOM트리와 data structure, 2가지 모두에서 제거해야 함  

```js
var elements = {
    image: document.getElementById('image')
};
function doStuff() {
    elements.image.src = 'http://example.com/image_name.png';
}
function removeImage() {
    // image는 body 요소의 바로 아래 자식임
    document.body.removeChild(document.getElementById('image'));
    // elements.image = null 을 통해 data structure에서도 image를 제거해줘야 함
}
```

image는 DOM트리와 elements라는 객체 data structure  
2가지 모두에서 제거되어야 한다  

DOM에서 제거했다고 하더라도  
data structure에서 참조하는 것을 제거하지 않는다면  
메모리 누수가 발생할 수 있다  

