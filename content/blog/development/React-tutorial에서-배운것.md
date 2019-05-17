---
title: React tutorial(JSX)
date: 2019-05-17 12:05:44
category: development
---
JSX는 React element를 생성  
React element란? DOM에 Render 될 객체이다.  

###JSX는 객체이다
Babel은 JSX를 React.createElement()를 호출하여 컴파일 함  

```js
const element = (
  <h1 className="greeting">
    Hello, React!
  </h1>
)
```

위와 같이 JSX를 element에 할당하면

```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, React!
)
```

바벨은 createElement()라는 method를 실행한다.  
이때 arguments로는  

- HTML tag의 이름
- classname등이 포함된 object
- children

이 있다.  

```js  
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, React!'
  }
}
```

createElement()는 위 object를 return한다.  
위 object는 JSX가 생성한 React element이다.  
그 구성은 아래와 같다.  

- type의 value값인 HTML tag이름
- props의 value값인 className과 children

###왜 JSX를 사용할까?
React는 렌더링 로직과 UI 로직이 분리될 수 없음을 고려하여 만들어진 라이브러리  
따라서 렌더링 로직 + UI 로직을 수행하는 Component라는 객체를 만듦  
이 Component는 React element로 구성됨  
JSX는 React elemet를 만드는 Markup Language의 일종  
Markup Language이므로 UI 로직을 시각적으로 이해하기 쉽게 표현(HTML tag와 모양 비슷)  
반드시 JSX를 사용하지 않아도 React 활용 가능(결국 React는 객체를 활용하므로)  
하지만 JSX를 사용하여 객체(element)를 만든다면 훨씬 이해하기 쉽고 빠르게 React 프로그래밍이 가능해짐  


