---
title: element, component와 props,state 개념 중간정리
date: 2019-05-23 13:05:82
category: React
---

- JSX : javascript 확장된 문법이다.(react element를 쉽게 표현하는)
  - JSX로 react element를 선언할 수 있다.
  - JSX 안에서 { } 를 활용하여 javascript 표현을 쓸 수 있다.
- element : UI를 나타내는 객체이다.(DOM element와 유사하다.)
  - HTML tag 종류, props, children을 property로 가진다.
  - ReactDOM.render()의 argument로 주어진다(그 결과 DOM element에 소속됨)
- component : pure function이다.
  - props를 argument로 받는다.(props값 수정하면 안됨 pure!)
  - element를 return 한다.
  - class 와 function 두가지로 선언 가능
  - lifecycle을 가진다.
- lifecycle : component의 생명주기이다.  
  - component가 return한 element가 DOM에 소속되는 시점에 따라 결정된다.
  - mount는 소속되는 시점, unmount는 제거되는 시점
  - componentDidMount, componentWillUnmount 등이 있다.
- props : element가 표시하는 data이다.
  - function component에게 argument로 주어진다.
  - class component는 this.props로 접근 가능하다.
  - 부모 component에서 자식 component로만(TOP-DOWN) 전달 가능하다.
  - component 내부에서 직접 수정이 불가능하다.
- state : component의 상태이다.(변하는 data를 처리하기 위해 필요)
  - props는 수정이 불가능하므로 state를 활용하여 변하는 data를 처리한다.
  - setState()를 활용해서 state 값을 수정한다.
  - setState는 비동기이다.(function을 argument로 전달하면 정확한 수정 가능)
  - setState()가 실행되면 component의 render()가 다시 실행된다.