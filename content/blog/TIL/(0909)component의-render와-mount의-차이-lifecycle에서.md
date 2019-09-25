---
title: (0909)component의 render와 mount의 차이 lifecycle에서
date: 2019-09-09 12:09:85
category: TIL
---

react는 어떻게 DOM을 수정해야 하는가를 파악한다  
component가 render 되길 원하는 모습과 DOM이 일치하도록

react는 mounting, unmounting, updating 3가지를 사용한다  
mounting은 React node를 DOM에 더하는 것이다  
unmounting은 React node가 DOM에서 제거되는 것이다  
updating은 이미 DOM에 추가된 React node의 data가 바뀌는 것이다

React node가 어떻게 DOM node로써 표현되는지,
언제 DOM tree에 추가되는지, DOM tree의 어디에 나타나는지  
이 모든 것은 top-level API 즉 `request('react')`에서 관리한다

## React CreateElement

```js
let foo = React.createElement(FooComponent)

foo = {
  type: FooComponent,
  props: {},
}
```

위처럼 foo에 React element를 할당하면  
foo는 DOM element는 아니다 DOM tree에 존재하지 않는다  
즉, 아직 `mount` 되지 않았다
단지 React에게 foo라는 element가 `render`되기 위해서  
무엇이 필요한지 말해줄 뿐이다

### ReactDOM.render

```js
ReactDOM.render(foo, domContainer)
```

render()가 호출되는 결과로  
root component 즉 domContainer에 foo를 mount 한다

부모 component가 setState()를 호출하거나  
render method를 호출하여 특정 자식 component를 render할 때  
React는 자동으로 자식 component를 부모 component에 mount 한다

### FooComponent의 instance 생성 및 render method 실행

FooComponent class 의 instance를 만들고 render method를 실행한다  
domContainer에게 mount된다

## Mount란?

1. React component(class)의 instance를 만든다
2. React component의 render method를 실행한다
3. React component와 일치하는 DOM node를 만든다
4. DOM container에 해당 DOM node를 삽입한다

위 과정을 mounting이라고 부른다
