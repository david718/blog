---
title: selection sort
date: 2019-08-28 15:08:09
category: Algorithms
---

selection sort는

1. 첫번째 item과 이후 item들을 비교한다
2. 가장 작은 item을 찾아 선택한다
3. 첫번째 item과 가장 작은 item을 swap한다
4. 두번째 item으로 넘어가서 위 과정을 반복한다

그 결과 가장 작은 item이 맨앞에 위치하는 과정을 반복하여  
작은 순서대로 정렬이 완료된다

##실제 코드

```js
function sel(arr) {
  for (let i = 0; i < arr.length; i++) {
    let minI = i
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[j] < arr[minI]) {
        minI = j
      }
    }
    if (minI !== i) {
      let change = arr[minI]
      arr[minI] = arr[i]
      arr[i] = change
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
