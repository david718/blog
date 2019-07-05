---
title: (0705)useRef를 활용한 props 값 저장
date: 2019-07-05 16:07:64
category: TIL
---

`useRef`를 활용하면 아래와 같은 `class component`에서  
time에 저장한 변하지 않는 data 즉, `props` 값을  
`function component`에서 활용할 수 있게 된다.

```js
class Excomponent extends Component {
  time = new Data()

  render () {
    return (
      <div>{this.time}</div>
    )
  }
}
```

function component에서 time을 활용하는 예시 코드

```js
const Excomponent = () => {
  const time = useRef(null)
  time.current = new Date();

  return (
    <div>{time.current}</div>
  )
}
```

위와 같이 `useRef`가 return한 객체의 속성 중  
`current` 에 저장하고 싶은 `props`를 저장하여 활용할 수 있다.

