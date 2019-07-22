---
title: (0719)hooks로 class component 기능 흉내내기
date: 2019-07-22 20:07:42
category: TIL
---

##useState
class component 에서 state 관리 기능을 대신하는 hooks 기능이다  

```js
const [state, setState] = useState('최초 state 값')
```

위 형식으로 functional component 안에서 state 관리를 가능하게 한다  

##useRef
class component 에서 특정 값을 `this`. 에 할당하여 활용하던 기능을  
대신하는 hooks 기능이다  
  
(정확히는 ref를 만드는 기능이다(이후 추가 공부 필요))  
  
```js
const something = useRef();

something.current = '할당하여 활용할 특정 값'
```

##useEffect
class component에서 lifecycle method 중

- componentDidMount()
- componentDidUpdate()
- componentWillUnmount()  

위 3가지를 전부 대체할 수 있는 hooks의 기능이다  

```js
useEffect(() => {
  /*<1:1 대응은 아님>
  componentDidMount: 두번째 인자인 배열에 아무것도 들어가지 않은 경우
  처음 함수형 component가 render될때 한번만 useEffect의 callback 인자가 실행된다

  componentDidUpdate: 두번째 인자인 배열에 바뀌는 값이 들어가면
  그 값이 바뀔때마다 useEffect의 callback 인자가 실행된다*/
  
  interval.current = setInterval(changeHand, 100);

  return ( // componentWillUnmount의 역할
    clearInterval(interval.current)
  )
}, ['여기 들어가는 state 값이 바뀔때마다 useEffect 에 인자로 주어진 callback이 실행'])
```

위에 예시가 동작하는 순서는 아래와 같다

1. functional component 전체가 실행된다
2. setInterval이 실행된다
3. return 안에서 clearInterval이 실행되어 setInterval이 지워진다
4. setInterval의 callback인 changeHand가 실행된다
5. state가 change된다(changeHand 안에서 setState 실행)
6. functional component 전체가 다시 실행된다
7. 다시 2번부터 반복

위 순서에 따라 동작하기 때문에 마치 setTimeout처럼 동작한다
  
반면, class component는 Re-rendering 될 때  
전체 component가 다시 실행되지 않는다  
(render method만 다시 실행된다)
