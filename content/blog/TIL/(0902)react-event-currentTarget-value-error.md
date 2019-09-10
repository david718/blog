---
title: (0902)react event currentTarget value error
date: 2019-09-03 11:09:21
category: TIL
---

class component안에서 form 을 작성할 때,  
아래와 같이 event 의 value를 setState 비동기 함수에 할당했다

```js
onChangeName = (e: any) => {
  this.setState(prev => {
    return {
      ...prev,
      username: e.currentTarget.value,
    }
  })
}
```

그러자 아래와 같은 에러를 console창에서 확인할 수 있었다

> This synthetic event is reused for performance reasons. If you're seeing this, you're accessing the property `target` on a released/nullified synthetic event. This is set to null.

event의 property를 직접 비동기 함수에 사용하면 발생하는 error다

##해결책: event.target.value를 변수에 할당하자

위 문제의 코드를 아래와 같이 수정했다

```js
onChangeName = (e: any) => {
  const value = e.currentTarget.value
  this.setState(prev => {
    return {
      ...prev,
      username: value,
    }
  })
}
```

그러자 error가 해결되었다

##왜? SyntheticEvent 의 pooling 현상

성능상의 이유로 SyntheticEvent는 pooling 현상이 있다  
즉 event 객체가 재사용되고 모든 속성은 event handler가 호출된 다음 초기화 된다
따라서 비동기적으로 event 객체에 접근할 수 없다

비동기적으로 event 객체에 접근하고 싶다면  
event.persist()를 호출하여 event 객체를 pool에서 제거해야 한다  
그럼 비동기적으로 event 객체에 접근이 가능해진다
