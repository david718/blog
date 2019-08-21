---
title: (0816)useEffect로 구현하는 componentDidmount, componentDidUpdate
date: 2019-08-21 17:08:62
category: TIL
---

useEffect는 hooks로써  
functional component에서 lifeCycle 활용을 가능하게 돕는다

##useEffect의 기본 사용

```js
useEffect(() => {
  //componentDidMount에 해당한다
  return () => {
    //componentWillUnmount 에 해당한다
  }
}, ['여기 요소가 변하면 체크하여 componentDidUpdate처럼 사용'])
```

##useEffect를 활용한 componentDidMount

```js
useEffect(() => {
  //여기에 필요한 로직 구현
}, [])
```

빈 배열을 2번째 인자로 주는 것이 포인트이다  
왜냐하면 어떠한 변화요소도 감지하지 않기 때문에  
component가 mount되는 최초 한번만 실행한다  
필요한 로직 구현 자리에 들어가있는 로직이 1번 실행된다

##useEffect를 활용한 componentDidUpdate

```js
const mounted = useRef(false)

useEffect(() => {
  if (!mounted.current) {
    mounted.current = true
  } else {
    //로직
  }
}, ['바뀌는 값'])
```

componentDidUpdate만 실행하고 싶을 때는 위 패턴을 사용한다

가장 먼저 실행될 때 즉 componentDidMount처럼 실행될 때는  
if 문의 안쪽이 실행된다(왜냐하면 `!mounted.current`가 `true`이기 때문에)

2번째 이후부터는 else 문의 안쪽이 실행된다  
그곳에 작성해 둔 필요한 로직이  
`바뀌는 값`을 체크해서 `componentDidUpdate`처럼 동작한다
