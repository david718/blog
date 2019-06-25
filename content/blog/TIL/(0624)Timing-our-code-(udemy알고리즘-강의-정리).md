---
title: (0622)Timing our code (udemy알고리즘 강의 정리)
date: 2019-06-22 14:06:50
category: TIL
---

time complexity와 space complexity 에 대한 개념 잡기  

##time complexity
무조건 for 문 범위를 보자  
범위가 고정되어있다면 O(1)이다
범위가 변수면 time complexity는 O(n)이다  
for 문 안에서 for 문 돌면 O(n^2) 이다  
 
##space complexity
space 란 memory를 말한다  
JS에서는 변수 type에 따라 차지하는 memory가 다르다  

- String : O(n) space (n = string length)
- Rest Primitives(boolean, number, undefined, null): constant space
- Reference type : O(n) space (n = Array length, the numbers of Object keys)

