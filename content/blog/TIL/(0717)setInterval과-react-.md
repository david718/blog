---
title: (0717)setInterval과 react 
date: 2019-07-22 11:07:91
category: TIL
---

componenetDidMount() 에서 비동기 요청을 한다  
즉 setInterval도 포함한다(이 라이프사이클에서 요청)  
  
componentWillUnmount() 에서 비동기 요청을 정리한다  
즉 clearInterval 등을 실행해서 요청했던 비동기 로직을 지운다  
(메모리 누수 방지)  
