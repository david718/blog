---
title: (0626)problem solving patterns(JS)
date: 2019-06-24 16:06:12
category: TIL
---

##Frequency counters(빈도수 세기)
빈도수를 셀 때는 object를 사용하자
세어야 할 item을 key, 빈도수를 value로 할당한다.  
object를 활용하여 빈도수를 세고 문제를 해결하는 전략

##multiple pointers
item을 가리키는 pointer를 설정하자  
pointer를 활용하여 문제를 풀자  
  
예시로 sorted Array를 받아 합이 0이 되는  
한쌍의 숫자를 return하는 함수 sumZero를 작성하자

```js
function sumZero(arr) {
  let start = 0;
  let end = arr.length - 1;
  while(start < end) {
    let sum = arr[start] + arr[end];
    if(sum === 0) {
      return [arr[start], arr[end]];
    } else if(sum > 0) {
      end -= 1;
    } else {
      start += 1;
    }
  }
}
```

위 코드에서 start와 end로 두 pointer를 설정했다  
이를 통해 조건을 걸고 pointer를 움직이며 문제를 해결했다  
이것이 multiple pointer 해결 전략이다  

##