---
title: (0819)useContext 와 createContext의 사용
date: 2019-08-19 11:08:14
category: TIL
---

contextAPI 를 사용하면 component 들 사이의  
props drilling 현상을 막을 수 있다

즉 최상위 component에서 최하위 component로  
한번에 props를 전달할 수 있다

useReducer와 함께 사용하면  
dispatch를 전달할 수 있게 되기때문에  
redux의 핵심기능들을 redux 없이 사용할 수 있게 된다

##createContext

```js
import { createContext } from 'react'

const newContext = createContext({
  state: value,
})
```

위처럼 `createContext`는 react 의 api이다  
새로운 context를 만드는 방법은  
`createContext`에게 전달할 props 를 포함한 객체를 인자로 주는 것이다

그럼 최하위 component는 어떻게  
`createContext`에 넣은 객체 안에 있는 state를 사용할 수 있을까?

##useContext

```js
import { useContext } from 'react'
import { newContext } from './TopComponent'

const { state } = useContext(newContext)
```

자식 component에서는 먼저 `useContext`라는 react api를 불러온다  
그 다음 최상위 TopComponent에 있는 newContext를 가져온다
`useContext`의 인자로 newContext를 넣으면  
`createContext`에 인자로 주었던 객체를 return한다

그 결과 구조분해할당으로 state를 불러와 사용할 수 있게된다
