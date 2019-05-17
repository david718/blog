---
title: React tutorial에서 배운것
date: 2019-05-17 12:05:44
category: development
---

# 1.JSX
JSX는 React element를 생성
React element란? 2.elements에서 설명

### JSX는 객체이다
Babel은 JSX를 React.createElement()를 호출하여 컴파일 함
```js{3}


```

### 왜 JSX를 사용할까?
React는 렌더링 로직과 UI 로직이 분리될 수 없음을 고려하여 만들어진 라이브러리
이를 고려하여 Component라는 객체를 만듦(렌더링 로직 + UI 로직)
이 Component는 element라는 객체로 구성되어 있음
JSX는 이 elemet를 만드는 Markup Language의 일종
Markup Language 즉 UI 로직을 시각적으로 이해하기 쉽게 표현(HTML tag와 모양 비슷)
반드시 JSX를 사용하지 않아도 React 활용 가능(결국 React는 객체를 활용하므로)
하지만 JSX를 사용하여 객체(element)를 만든다면 훨씬 이해하기 쉽고 빠르게 React 프로그래밍이 가능해짐


