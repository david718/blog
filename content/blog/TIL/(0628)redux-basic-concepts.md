---
title: (0628)redux basic concepts
date: 2019-06-28 22:06:73
category: TIL
---

(벨로퍼트 Redux 기초 공부하며 정리한 것)  
  
Redux는 상태 관리 라이브러리다.  
action, reducer, store, dispatch 등의  
개념들로 구성되어 있다.  

##action
상태에 변화가 필요할 때, action을 발생시킨다.  
action은 객체로 표현된다.  
객체에는 type이라는 key와 value값이 반드시 있어야 한다.  

```js
{
  type: "CHANGE_INPUT",
  text: "안ㄴ"
}
```

##action creator
action 생성 함수는 data를 action으로 만드는 함수이다.  
상태의 변화값이 될 data를 input으로 받아서  
action 객체를 return한다.  

```js
const changeInput = text => ({ 
  type: "CHANGE_INPUT",
  text
});
```

##reducer
reducer 는 변화를 일으키는 함수이다.  
현재 state와 action을 input으로 받아서  
바뀐 state를 return한다.  

```js
const reducer = (state, action) {
  //상태 업데이트 로직

  return changedState;
}
```

##store
react app에서 1개만 존재하는 store는  
현재 app의 state와 reducer가 있다.  
또한, 몇 가지 내장 함수도 있다.  
아래 2가지를 소개한다.  

###dispatch
dispatch는 store의 내장 함수 중 하나이다.  
dispatch는 action을 발생시키는 함수이다.  
  
1. dispatch(action) 을 실행시킨다
2. store는 reducer를 실행시킨다
3. 해당 action을 처리하는 로직이 있다면 실행된다
4. 로직이 action을 참조하여 state를 바꾼다

###subscribe
subscribe도 store의 내장 함수 중 하나이다.  
subscribe는 함수를 input으로 받는다.  
action이 dispatch 되었을 때마다 input으로 받은 함수가 호출된다.  
