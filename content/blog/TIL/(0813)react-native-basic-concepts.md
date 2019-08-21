---
title: (0813)useMemo의 사용
date: 2019-08-20 09:08:36
category: TIL
---

```js
const lottoNumbers = useMemo(() => getWinNumbers(), [])
```

위처럼 useMemo에는 callback과 배열이 인자로 주어진다

이때 callback은 배열의 요소가 바뀔때만 실행된다  
만약 빈배열이어서 어떠한 요소의 변화도 체크하지 않거나  
혹은 체크하는 배열의 요소가 바뀌지 않는다면  
callback은 실행되지 않는다

##functional component에서 useMemo의 필요성
functional component는 state 혹은 props가 바뀌는 경우  
전체 함수가 재실행된다

그러므로 component 안에서 특정 함수를 실행하는 경우  
전체 함수가 재실행될때마다 특정 함수도 함께 재실행되는 낭비가 발생한다

이를 방지하기 위해 useMemo 를 활용한다

##useMemo의 실제 사용예시

```js
useMemo(() => {
  ///특정 함수
}, ['이 안에 요소가 바뀌는지 체크'])
```

useMemo를 활용하는 방법은 아래와 같다

1. 특정 함수 자리에 특정 함수를 작성한다
2. 배열 안에 변화를 체크할 요소를 넣는다

그 결과 요소가 변화할 때만 callback이 실행되어  
특정함수가 실행된다
