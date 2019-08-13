---
title: (0810)css img 혹은 text(한줄)을 세로 가운데 정렬하는법(styled-component 사용)
date: 2019-08-10 14:08:77
category: TIL
---

styled-component를 사용하여 tag를 세로 가운데 정렬하는 방법을 보자  
img와 text 2가지 경우 모두 상위 tag가 필요하다

##상위 tag
상위 tag에는 line-height 라는 속성을 사용하여  
그 값은 height 값과 동일하게 할당한다

```js
const Container = styled.div`
  height: 60px;
  line-height: 60px;
`
```

위처럼 `div`를 활용하여 상위 tag의 css를 지정한다
그 후 span tag를 이용하여 text를 넣으면 세로 가운데 정렬이 된다

##text

```js
const Text = styled.span`
  ~~~
`
```

span 안에 담긴 text 요소는 추가로 css가 필요하지 않다  
세로 가운데 정렬이 되어있음을 확인할 수 있다

##image

```js
const Img = styled.img`
  vertical-align: middle;
`
```

verticla-align 이라는 img 의 속성값으로 middle을 준다  
이제 img도 세로 가운데 정렬이 되었음을 확인할 수 있다
