---
title: (0622)css declaration 우선순위
date: 2019-06-22 16:06:95
category: TIL
---

css의 문법에 대해 새롭게 안 지식을 기록하자

##attribute
자주 쓰이는 attribute 부터 공부하자

###font-size
글자 크기를 단위로 표현한 특성  
  
단위의 종류는 3가지

- px(절대적)
- em(상대적)
- **rem**(상대적)

사용자가 브라우저의 글꼴 크기를 _키웠을 때_  

- px은 바뀌지 않는다
- rem은 바뀐다(따라 커진다)

rem은 html tag의 font-size 값에 비례한다  

###color
글자 색을 표현법으로 지정한 특성

표현법의 종류는 3가지

- color name
- hex(rgb를 16진법으로 표현)
- rgb

###text-align
값은 center, left, right 가 있다  
  
justify는 해당 tag에 꽉차게 배열하는 속성 값이다.  
자간을 넓혀 꽉차게 만든다  

###font

- font-family: 글꼴 지정(serif 장식(삐쭉삐죽) 있음, sans-serif 장식 없음)
- line-height: 줄 간격
- font: font의 다양한 옵션들을 한꺼번에 쓸 수 있음(순서지켜야 함)

##css 적용 규칙
다양한 tag의 영향을 받기 때문에 css를 적용하는 규칙이 필요하다  
여러 css가 중복되어서 적용되었을 때  
어떠한 css를 우선 적용할지 기준이 필요하기 때문이다  
  
2가지 규칙이 있다

- inheritance
- cascading

먼저 규칙에 대해 알아보기 전에 규칙을 무시하는 방법이 있다  
무조건 내가 작성한 css가 최우선으로 적용되게 하는 방법

> css 문구 뒤에 **!important** 라고 쓰면 해당 css 문구가 최우선으로 적용된다

그럼 이제 2가지 규칙에 대해 알아보자

###inheritance
어떤 html tag에 property로 css를 스타일링 하게 되면  
해당 html tag에 속한 자식 tag들이 같이 css를 적용받게 되는 규칙이다  
  
inheritance 되는 속성과 안되는 속성이 나뉘어져 있다  

###cascading
css의 c인 cascading 은 css의 철학을 표현한다  
폭포라는 뜻으로 웹브라우저 + 사용자 + 제작자의 스타일링 의도를  
모두 모아서 스타일링 한다는 철학을 의미한다  
(폭포처럼 모두의 디자인 의도가 한 곳에 모여 만드는 걸 의미)  
  
이러한 조화로운 스타일링을 위해서는 적용 우선순위가 필요하다  
  
기본적으로 우선순위는 `웹브라우저 < 사용자 < 저자` 이다
  
그럼 저자 즉 개발자가 css를 적용할 때 그 안에서 우선순위는?  
구체적인 selector일 수록 선언의 적용 우선순위가 높다.  

1. style attribute
2. id selector
3. class selector
4. tag selector

위와 같은 우선순위를 따라 html tag에 css가 적용된다.  
가장 구체적으로 html tag를 섬세하게 선택한 것이  
먼저 적용되는 기준을 따른다.  
  
> 이 규칙을 깨는 것이 **!important**이다(이걸 css 구문 마지막에 붙여주면 무조건 최우선으로 적용된다)

