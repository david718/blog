---
title: insertion sort
date: 2019-09-13 21:09:59
category: Algorithms
---

첫 item부터 마지막 item까지  
앞서 정렬된 array에 순서에 맞게 insertion(삽입)하는 방식이다
최종적으로 모든 array가 정렬될때까지 item을 insertion한다

`[1,2,3,5,9,6,7]` 같은 거의 정렬 되어있지만  
한가지 정렬 안된 item을 가진 array의 경우  
insertion sort 를 활용하기에 유리하다

또한 길이가 실시간으로 변하는 data set의 경우도 유리하다

##pseudo code

1. 첫번째 item을 선택한다(이미 정렬됨)
2. 2번째 item을 이미 앞서 정렬된 array에 순서에 맞게 insertion(삽입)한다
3. 마지막 item까지 순서에 맞게 insertion하여 정렬을 완료한다

##code

```js
function insertionSort(arr) {
  for (let i = 1; i < arr.length; i++) {
    let currentValue = arr[i]
    for (var j = i - 1; j >= 0; j--) {
      if (arr[j] > currentValue) {
        arr[j + 1] = arr[j]
      } else {
        break
      }
    }
    arr[j + 1] = currentValue
  }
  return arr
}
```
