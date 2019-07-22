---
title: (0717)setInterval을 react에서 사용하기 & callback 처리 pattern
date: 2019-07-22 11:07:91
category: TIL
---

componenetDidMount() 에서 비동기 요청을 한다  
즉 setInterval도 포함한다(이 라이프사이클에서 요청)  
  
componentWillUnmount() 에서 비동기 요청을 정리한다  
즉 clearInterval 등을 실행해서 요청했던 비동기 로직을 지운다  
(메모리 누수 방지)  

##callback 처리 pattern

```js{8}
const test = () => {
  const handleClick = (variable) => {
    return variable;
  }

  return (
    <div>
      <button onClick={(e) => handleClick(variable)}>테스트</button>
    </div>
  )
}
```

위와 같은 경우 onClick에 할당되는 부분(하이라이팅된 부분)에  
callback이 계속 새로 선언되고 있다(새로운 함수가 계속 생성)  
그 결과 메모리 누수가 발생한다  
  
메모리 누수를 막기 위해 아래와 같은 callback 패턴을 사용한다  

```js{2, 8}
const test = () => {
  const handleClick = (variable) => (e) => {
    return variable;
  }

  return (
    <div>
      <button onClick={handleClick(variable)}>테스트</button>
    </div>
  )
}
```

위와 같은 패턴으로 callback을 선언하게 되면  
handleClick은 함수를 return하는 함수가 된다  
  
따라서 handleClick의 실행 결과는 함수이므로  
onClick에 함수를 할당하는 결과를 가져오게 된다  
(즉 이전에 함수를 새롭게 선언해서 할당하는 것과 같은 결과)
