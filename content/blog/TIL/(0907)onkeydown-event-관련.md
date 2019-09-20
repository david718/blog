---
title: (0907)onkeydown event 관련
date: 2019-09-20 13:09:63
category: TIL
---

onkeydown event에 대한 handler 를 작성할 때  
주의할 점은 tabIndex 이다

onkeydown 은 HTML tag 의 속성이다
마찬가지 tabIndex 또한 HTML tag 의 속성이다

onkeydown의 handler 에서  
keypress event를 받기 위해서는  
반드시 해당 tag에 tabIndex 값을 할당해주어야 한다
