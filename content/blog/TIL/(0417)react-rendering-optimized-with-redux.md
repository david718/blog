---
title: (0417)react rendering optimized with redux
date: 2020-04-17 16:04:24
category: TIL
---

1. what is rendering optimization
2. when react component re-rendering
3. when unnecessary rendering happen
4. how prevent unnecessary rendering
   - useCallback
   - useMemo
   - React.memo

## 1. what is rendering optimization

rendering optimization 이란  
`불필요한 re-rendering` 을 방지하는 것이다

react component는 data(props, state 등)을  
보여주기 위해 rendering 이 된다  
즉 보통의 경우, data 가 바뀌지 않으면 re-rendering 할 필요가 없다

그러므로 `불필요한 re-rendering` 이란  
component의 data 가 바뀌지 않았는데 발생하는 re-rendering을 말한다

그렇다면 component 는 언제 re-rendering 이 발생할까

## 2. when react component re-rendering

- props change
- state(in component) change
- parent component rendering
- shouldComponentUpdate return true
- forceUpdate invoke

위 5가지 경우에 component 는 re-rendering 된다  
아래 두 경우(should~, force~)는 rendering을 control하는 method이다  
따라서 `불필요한 re-rendering` 이 언제 발생하는지와는 무관하다
`불필요한 re-rendering` 을 방지하는 용도로 사용한다

## 3. when unnecessary rendering happen

`불필요한 re-rendering` 이 발생하는 대표적인 경우를 다루려 한다

### props value not change but re-rendering

그 경우는 props 의 실제 value 는 변하지 않았는데  
re-rendering 이 발생하는 경우이다

props 로 전달하는 value 의 종류는 2가지로 구분한다

- primitive type : string, number, boolean 등
- reference type : function, array, object 등

reference type 의 경우 실제 value 와 그것이 저장된  
reference 의 value 로 나누어서 이해해야 한다  
즉, 선언하면 reference value 가 생성되고 그 안에  
실제 value 가 담겨있다

이러한 reference type 을 props 로 전달할 때는  
reference value 를 전달한다  
따라서 reference type 이 다시 선언되면  
실제 value 는 변하지 않지만 reference value 는 변하므로  
자식 component는 새로운 props 로 인식한다  
props 로 전달하는 reference value 가 변했기 때문이다  
이러한 props change 는 re-rendering 을 발생한다

## 4. how prevent unnecessary rendering

이렇게 `불필요한 re-rendering` 을 방지하기 위해서는  
먼저 props로 전달하는 reference value 가 바뀌지 않도록 해야한다  
이를 위해 기존에 전달하는 reference value 를 저장하는 방법이 있다

- useCallback : event handler 등 callback 에 활용
- useMemo : array 등 data 에 활용

위 방법은 reference 에 저장된 사용값(callback, data 등)이 변하지 않았다면  
기존에 저장한 reference value 를 props로 전달한다  
따라서 자식 component 는 새로운 props로 인식하지 않는다  
그러므로 re-rendering 이 발생하지 않는다

구체적인 위 방법의 활용 예시를 살펴보자

### useCallback

https://ko.reactjs.org/docs/hooks-reference.html#usecallback

```js
const memoizedCallback = useCallback(() => {
  doSomething(a, b)
}, [a, b])
```

useCallback 에 첫번째 인자로 주어진 callback function 은  
reference value 가 저장된다  
두번째 인자로 주어진 배열 안에 값들(a,b)이 변할 때만  
이 reference value 는 변한다(다시 실행되어 새롭게 선언되므로)

따라서 a,b 가 변하지 않는다면  
기존에 저장해둔 reference value 를 props 로 전달한다  
그러므로 props change 가 발생하지 않고  
그 결과 `불필요한 re-rendering` 을 방지할 수 있다

### useMemo

```js
const memoizedValue = useMemo(() => ['something'], [a, b])
```

useMemo 에 첫번째로 주어진 callback 이  
return 하는 값(array 등)을 저장한다  
두번째로 주어진 array의 element 값(a,b)이 바뀌지 않는 한  
기존에 저장한 return 값을 새롭게 저장하지 않는다

저장해둔 값을 props로 전달하므로 props change 가 없다  
그 결과 `불필요한 re-rendering` 을 방지할 수 있다

## React.memo

https://ko.reactjs.org/docs/react-api.html#reactmemo

function component 를 인자로 받는 `React.memo`는  
function component 의 return 값을 저장(memoization)한다  
props change 가 발생하지 않으면 function component 를 호출하지 않고  
기존에 저장해둔 return 값을 전달한다

이로써 function component의 re-rendering 뿐만 아니라  
호출 또한 방지할 수 있게 된다

> 주의 점은 `React.memo` 내부에서  
> useState, useContext, useSeletcor 호출시  
> (state 나 context(redux) 가 변할 때) re-rendering 된다

따라서 re-rendering 을 방지하기 위해 React.memo 를 적용한  
function component 내부에서는  
useState, useContext, useSelector 등은 사용하지 않는다

## reference

- https://ko.reactjs.org/
