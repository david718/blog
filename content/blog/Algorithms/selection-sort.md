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

위 코드에서 볼 수 있듯이
가장 작은 item의 index를 `minI`에 할당하였다  
만약 비교대상인 첫번째 item보다 작은 item이 있다면  
즉, minI !== i 라면 해당 작은 item과 첫번째 item을 swap한다
그렇게 마지막 item까지 모두 비교하여 정렬한다

##selection sort의 특징
반복문을 1번 돌 때 swap이 1번 일어난다  
(만약 비교하는 item보다 더 작은 item이 없다면 swap이 일어나지 않는다)

이러한 특징 때문에 비교 연산을 최소 실행하고 싶을 때  
selection sort가 유용하다  
(bubble sort는 모든 item에 대해 비교연산을 실행한다)
