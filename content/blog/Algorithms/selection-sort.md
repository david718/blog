---
title: selection sort
date: 2019-08-28 15:08:09
category: Algorithms
---

selectionSort는 가장 작은 item을 선택하여  
정렬 중인 배열에 맨 앞에 두는 방식으로  
끝까지 정렬해나가는 정렬 방식이다

`[1,2,3,4,7,8,0,9]` 과 같이 거의 정렬되어 있고  
가장 작은 값이 중간에 있는 배열의 경우  
selection Sort를 활용하면 효율적으로 정렬할 수 있다

##psuedo code

1. item을 선택한다
2. 뒤에 있는 item과 하나씩 비교한다
3. 그 중 가장 작은 item과 자리를 바꾼다
4. 처음 선택한 item이 가장작다면 그대로 둔다
5. 마지막 item까지 선택하여 위과정을 반복한다

##실제 code

```js
function selectionSort(arr) {
  for (let i = 0; i < arr.length; i++) {
    let minI = i
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[j] < arr[minI]) {
        minI = j
      }
    }
    if (minI !== i) {
      let change = arr[i]
      arr[i] = arr[minI]
      arr[minI] = change
    }
  }
  return arr
}
```
