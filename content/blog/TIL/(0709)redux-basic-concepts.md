---
title: (0709)redux basic concepts
date: 2019-07-12 14:07:36
category: TIL
---

App의 모든 상태는 하나의 store에서 관리한다.  
store에 저장된 상태 즉, state를 바꾸기 위해서는  
action 객체를 dispatch로 전달하는 방법 뿐이다.  
실제 코드로는 아래와 같이 구현한다.  

##REDUX 코드 구현

```js
import { createStore } from 'redux'
```

먼저 createStore를 가져와서 store를 만들 준비를 한다
그리고 아래와 같이 reducer를 선언한다

###reducer

```js
function counter(state = 0, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}
```

`reducer`란 **pure function**이다  
(state, action) => state 의 형태  
즉, 기존의 state와 action을 input으로 받아 새로운 state를 return한다  
**pure function**이므로 __같은 input__이라면 __언제나 같은 output__을 만든다  
  
state의 형태는 다양하다 array, object 등  
state는 직접 수정할 수 없다  
수정값을 포함한 새로운 state를 만드는 방식으로 수정한다  
  
위 `reducer`는 switch 구문을 사용했지만 fuction maps도 가능하다

###store

```js
let store = createStore(counter)
```

store를 만들기 위해 createStore에 reducer를 input으로 준다
store는 subscribe, dispatch, getState 3가지 API를 가진다

###3가지 API

```js
store.subscribe(() => console.log(store.getState()))

store.dispatch({ type: 'INCREMENT' })
// 1
store.dispatch({ type: 'INCREMENT' })
// 2
store.dispatch({ type: 'DECREMENT' })
// 1
```

- subscribe: 상태가 변하면 UI를 업데이트하기 위해 사용하는 API(보통 subscribe대신 react 등 UI 라이브러리를 활용)
- getState: 현재 상태를 가져올 수 있는 API
- dispatch: state를 수정하는 API

###dispatch
dispatch는 action을 전달하여 결국 state를 수정한다

```js
store.dispatch(action)
```

1. action을 받아서 reducer에게 전달한다  
2. reducer안에 actionType에 맞는 함수를 실행한다  
3. 새로운 state 객체를 return한다
4. store안에 state를 새로운 state로 수정한다

###action
action은 dispatch에 전달되어 state를 수정하는 사용자 행동이다  
click이나 무엇을 추가하거나 빼거나 등의 행동이 있다  
  
action은 또한 plain object이다  
plain object란? 아래 2가지 방법으로 만든 object를 말한다

- {} 즉, literal
- new Object()

constructor 로 만든 object와 다르게 property를 공유할 수 없다
