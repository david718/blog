---
title: 1.JSX[React Tutorial]
date: 2019-05-17 12:05:44
category: React
---
JSX는 React element를 생성한다.  
React element란? DOM에 Render 될 객체이다.  

##왜 JSX를 사용할까?
React는 렌더링 로직과 UI 로직이 분리될 수 없음을 고려하여 만들어진 라이브러리이다.  
따라서 렌더링 로직 + UI 로직을 수행하는 Component라는 객체를 만든다.  
이 Component는 React element로 구성된다.  

> JSX는 React elemet를 만드는 Markup Language의 일종

Markup Language이므로 UI 로직을 시각적으로 이해하기 쉽게 표현한다.  
**-> HTML tag와 모양 비슷**  
  
반드시 JSX를 사용하지 않아도 React 활용 가능하다.  
**-> React.createElement()를 통해 객체(element) 생성 가능하므로**  
  
하지만 JSX를 사용하여 객체(element)를 만든다면  
훨씬 이해하기 쉽고 빠르게 React 프로그래밍이 가능해진다.  

##JSX는 Injection Attacks로부터 안전하다
아래와 같이 사용자 input을 JSX안에 넣는 것도 안전하다.

```js
const title = response.potentiallyMaliciousInput;
//This is safe
const element = <h1>{title}</h1>;
```

React DOM은 렌더링하기 전에 JSX안에 넣은 모든 값을 _escape_ 한다.  
모든 것은 렌더링되기 전에 string으로 변환한다.  
_XSS 공격 방지_

##JSX represents object
JSX는 object(React element)를 나타낸다.  

```js
const element = (
  <h1 className="greeting">
    Hello, React!
  </h1>
)
```

위와 같은 JSX 코드를, Babel이 컴파일하면 아래 코드가 실행된다.  

```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, React!
)
```

Babel은 JSX를 컴파일하기 위해  
위 코드처럼 React.createElement()를 실행하고  
이때 arguments로는  

- HTML tag의 이름
- classname등이 포함된 object
- children

이 들어간다.  

```js  
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, React!'
  }
}
```

Babel이 컴파일한 결과 위 object가 생성된다.  
즉, React.createElement()가 실행된 결과로  
element 변수에 위 object가 assign된다.  
이 object가 바로 **React element**이다.  
> React element = JSX가 나타내는 것 = React.createElement()의 return값

React element의 구성은 아래와 같다.  

- type의 value값인 HTML tag이름
- props의 value값인 className과 children
  
다음 블로깅에서는 React element를 DOM에 렌더링하는 방법을 살펴보자