---
title: (0706)react component의 실행 순서
date: 2019-07-06 14:07:53
category: TIL
---

lifecycle을 고려하여 react component 함수의  
실행 순서를 살펴보자  

##class component

1. constructor(method와 props, state를 선언하는 과정)
2. render
3. ref
4. `componentDidMount`
5. setState / props 바뀌면 `shouldComponentUpdate` 실행
6. `shouldComponentUpdate`가 return true 하면 render(재실행)
7. `componentDidUpdate`
8. 부모가 나를 없애거나 DOM에서 제거될 때 `componentWillUnmount`
9. 소멸

`lifecycle method`는 component 생애주기에 맞춰  
로직을 실행시킬 수 있도록 실행 시기를 정의한다.  
