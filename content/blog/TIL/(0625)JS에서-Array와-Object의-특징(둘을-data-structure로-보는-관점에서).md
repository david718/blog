---
title: (0625)JS에서 Array와 Object의 특징(둘을 data structure로 보는 관점에서)
date: 2019-06-24 14:06:21
category: TIL
---

##Object

object는 아래 2가지 경우에 사용한다

- 순서 상관 없이 data를 관리할 때
- 빠르게 접근, 저장 혹은 삭제하고 싶을 때

###object에서 search하려면 O(n)이다
key에는 바로 접근 가능하다  
하지만 key의 value에는 key를 통해서만 접근할 수 있다  
따라서 원하는 value를 찾기 위해서는  
key마다 돌면서 value를 확인해야 한다  
따라서 key 개수만큼 시간복잡도가 증가하므로 O(n)이다

##Array

array는 순서가 있는 data structure이다  
즉 item마다 순서에 따라 index가 부여되어 있다
index가 key 역할을 한다  
  
따라서 처음이나 중간에 item을 추가할 때  
모든 item의 index가 다시 할당되어야 한다  
그러므로 추가하는 시간복잡도는 O(n)이다

###Array의 sort () method는 O(N * logN)이다
자세한 내용은 sorting algorithm 할 때 알아보자