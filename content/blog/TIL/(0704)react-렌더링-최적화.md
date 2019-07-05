---
title: (0704)react lifecycle method와 component 렌더링 최적화
date: 2019-07-05 11:07:14
category: TIL
---

react에서 렌더링 최적화란?  
`필요할 때`만 component가 렌더링 되도록 하는 것.  
  
그럼 `필요할 때`는 언제일까?  
component가 렌더링 할 data 값이 변할 때 이다.  

`필요할 때` 즉, data가 변할 때만 다시 렌더링되도록  
어떻게 component를 제한할 수 있을까?  

##lifecycle method
component는 lifecycle을 갖게 된다.  
말그대로 component가 태어나고 죽는 것이다.  
  
- 태어난다 = DOM에 소속된다  
- 죽는다 = DOM에서 제거된다

이러한 lifecycle 안에서  
component가 시기에 맞춰 필요한 동작을 수행할 수 있도록  
lifecycle method를 사용할 수 있다.  
  
오늘 기록할 것은 3가지 lifecycle method이다.  

1. shouldComponentUpdate
2. componentDidMount
3. componentWillUnmount

###shouldComponentUpdate()
`props`와 `state`가 변하기 직전에 호출된다.  
따라서 data가 변할 때만 다시 렌더링 되도록 component를 제한할 수 있다.  
  
component에 주어지는 data 즉 `props`와 `state`가 변하는  
조건에 대한 로직을 작성하고 **true** or **false** 로 __return__한다
  
true를 return하는 경우 `getSnapshotBeforeUpdate` method가 실행된다  
즉 변할 때만 component가 렌더링이 다시 될 수 있도록 제한할 수 있다.  
  
리액트가 렌더링되는 순서는  

1. **virtual DOM**에 렌더링됨
2. **virtual DOM**의 변화를 감지하여 **DOM**에 변화된 부분 다시 렌더링됨

을 거치게 된다.  

이때 `shouldComponetUpdate`를 사용하게 되면  
virtual DOM에 렌더링될 때, data가 변한 component만 렌더링할 수 있게 된다.  

###componentDidMount()
이름 그대로 Mount된 후 실행 즉, component 렌더링 시에 호출됨  
Mount란 component가 실제 DOM에 소속되는 것을 말함
주로 아래 3가지 경우 사용된다  

- 외부 라이브러리 연동(DOM 직접 사용하는 차트 라이브러리 사용 시 등)
- component에 필요한 data를 ajax 요청
- DOM 관련 작업(cross browsing 처리 위해 DOM정보 읽어올 때)

###componentWillUnmount()
Mount는 component가 실제 DOM에 소속되는 것을 말하므로  
unmount 즉, component가 DOM에서 제거될 때를 말함  

보통 component에 등록된 event를 제거하는 로직이 들어감  

