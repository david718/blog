---
title: (0604)react project tips(ref 등)
date: 2019-06-04 14:06:94
category: TIL
---

제로초 리액트 웹게임 강의 듣기 시작했다.  
강의 중에 나오는 개념들을 정리했다.  

##webpack
쪼개진 javascript 파일들을 하나로 합쳐주는 기능을 한다.  
쪼개진 javascript 파일이 모듈이다.  

##react 
쪼개진 react 파일들은 webpack으로 하나로 합친 후  
html에서 script 태그안에서 불러온다.  
  
cdn으로 html안에 `react.js` 삽입 가능하다.  
`react-dom.js`는 react를 web에다가 렌더링해주는 역할. 역시 삽입 가능.  

###React.createElement

```js{2}
React.createElement(
  type,
  [props],
  [...children]
)
```

type에는 HTML tag, React component, React fragment가 들어갈 수 있다.  

###JSX
`JSX`는 `React.createElement`를 대체하기 위해 만들어졌다.  
`JSX`를 호출하는 코드를 `Babel`(javascript 컴파일러)이 만나면  
`React.createElement()`를 실행한다.  

###Babel
`<script src >` 로 `Babel`도 마찬가지로 불러올 수 있음.  
불러온 후, 컴파일 할 `<script>`의 내용에 대해서  
 `<script>`의 `type속성` 값으로 `"text/babel"`를 주면 해석된다.
 뭘 해석하냐? JSX등 실험적인 javascript 문법을 해석한다.  
 
###ref

```js
class Something extends React.Component {
  onSubmit = (e) => {
    e.preventDefault();
    this.selectElement.focus();
  }

  selectElement;

  <input ref={(c) => this.selectElement = c;} />
}
```

ref의 사용은 위 코드에서 확인할 수 있다.  

1. ref에 callback을 할당한다.  
2. callback 안에서 class에 선언한 객체를 불러온다.
3. 불러온 객체에게 callback의 인자를 할당한다.
4. 이후 인자를 호출하면 ref 값을 준 tag를 직접 참조할 수 있다.

```js
const inputRef = React.useRef(null);

<input ref={inputRef} />
```

hooks에서는 ref를 위 처럼 사용한다.  

###setState 함수 넣기

```js
this.setState((prevState) => {
  return {
    state: 'newState'
  }
})
```
  
이전 state 값을 사용해서 state 값을 업데이트 하는 경우  
위처럼 함수를 인자로 넣어주어야 한다.  
setState가 비동기이기 때문이다.  
  
왜 비동기냐?  
state 업데이트 될때마다 렌더를 다시하므로  
최적화를 위해 state 업데이트를 한꺼번에 모아서 처리한다.  


###hooks
hooks는 state가 바뀔 때마다 hooks 함수 전체가 다시 실행된다.  
그 결과 안에서 선언한 method들이 전부 새로 만들어진다.  
최적화 필수!

###for 과 class(HTML tag attribute)
HTML에서 tag의 속성인 for과 class는  
React에서는 htmlFor과 className으로 대체한다.  
(이름이 겹치기 때문에)

