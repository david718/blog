---
title: quick sort
date: 2019-09-18 10:09:47
category: Algorithms
---

quick sort는 random 한 item을 기준으로  
크기를 비교하여 나열하고 나열된 부분 array에 대해  
앞의 과정을 반복하여 정렬하는 방식이다

대충 정렬하고 쪼개고 대충 정렬하고 쪼개고의 반복이다

여기서 대충 정렬이란 기준 삼은 item 과 크기를 비교하여 나열하고  
서로의 크기를 비교하여 완벽한 정렬은 하지 않는 것을 말한다  
대충 정렬의 결과로 기준 item이 바른 위치에 정렬된다

즉, 쪼개고 기준 item 정렬하고 쪼개고 기준 item 정렬하고  
반복하여 모든 array의 item을 정렬하는 방식이다

## pseudocode

1. 전체 array 중 random 한 item을 기준삼는다
2. 기준 item보다 작은 것은 왼쪽으로 큰 것은 오른쪽으로 나열한다
3. 나열된 왼쪽 부분 array 중 random한 item을 기준삼는다
4. 다시 작은 것은 왼쪽으로 큰 것은 오른쪽으로 나열한다
5. 왼쪽 부분 array의 item이 1개일 때까지 위 과정을 반복한다
6. 마찬가지 오른쪽 array의 item이 1개일 때까지 반복한다
7. 모든 item이 정렬된다

위 과정을 보면 알 수 있듯이 대충 정렬하는 과정  
즉, 기준 item과 크기 비교하는 과정을 위한 function과  
쪼개고 반복하는 recursion을 위한 logic이 분리되어야 한다

즉 2가지 단계가 필요하다

- 대충 정렬 function : pivot
- recursion logic

## 대충 정렬 function : pivot

pivot 이라는 function을 통해 기준 item과 크기를 비교하여  
array를 대충 정렬해보자
이 function은 기준이 되는 item의 정렬된 index 를 return한다

```js
function pivot(arr, start = 0, end = arr.length) {
  let pivotIndex = start

  for (let i = start; i < end; i++) {
    if (arr[start] > arr[i]) {
      pivotIndex++
      let change = arr[i]
      arr[i] = arr[pivotIndex]
      arr[pivotIndex] = change
    }
  }
  let change = arr[start]
  arr[start] = arr[pivotIndex]
  arr[pivotIndex] = change

  return pivotIndex
}
```

위 함수를 보면 arr외에 start 와 end 를 추가로 받는다  
왜냐하면 recursive하게 실행되어야 하기 때문이다  
즉, 반복실행되다가 특정 start와 end 값에서 실행이 멈춰야하기 때문이다

그럼 반복 실행하는 quickSort function을 이어서 살펴보자

## quickSort function

```js
function quickSort(arr, left = 0, right = arr.length) {
  if (left < right) {
    let pivotIndex = pivot(arr, left, right)
    quickSort(arr, left, pivotIndex)
    quickSort(arr, pivotIndex + 1, right)
  }
  return arr
}
```

위 함수를 보면 quickSort 안에서 quickSort를 호출하고 있다  
이것이 recursion이다

이때 인자로 주는 left 와 right의 값이 계속 바뀐다  
left 와 rigth 가 계속 바뀌다가  
left 가 right 보다 커지면 즉, `left < right`가 아닌 경우  
(여기서는 `left === right`가 되는 경우)  
quickSort 는 arr를 return 하게 된다

`left === right`라는 조건의 의미는  
arr의 item이 하나 남았다 즉, 정렬 완료라는 뜻이다

### let pivotIndex = pivot(arr, left, right)

가장 먼저 `let pivotIndex = pivot(arr, left, right)`를 보자
여기서 pivot을 실행하여 기준 item을 통해 전체 array를 대충 정렬하였다
즉, pseudocode의 2번에 해당한다

### quickSort(arr, left, pivotIndex)

다음 첫번째 quickSort인
`quickSort(arr, left, pivotIndex)`의 인자를 보면  
pivotIndex 가 right값으로 주어진 것을 확인할 수 있다  
즉, pivot을 실행하여 대충 정렬한 후  
대충 정렬한 전체 array의 왼쪽 array를 다시 정렬하기 위해  
`quickSort(arr, left, pivotIndex)`를 호출했음을 알수 있다
이 과정은 pseudocode의 3번에 해당한다

### quickSort(arr, pivotIndex + 1, right)

두번째 quickSort인
`quickSort(arr, pivotIndex + 1, right)`의 인자를 보면  
pivotIndex + 1 이 left값으로 주어진 것을 확인할 수 있다
대충 정렬한 전체 array의 오른쪽 array를 다시 정렬하기 위함이다
이 과정은 pseudocode의 6번에 해당한다

### 정렬 완료 return arr

이 모든 과정이 recursive하게 실행되고 종료되면  
최종적으로 정렬이 완료된 arr를 return하게 된다

## Big O of quickSort

| Time Complexity(Best) | Time Complexity(Ave) | Time Complexity(Worst) | Space Complexity |
| :-------------------: | :------------------: | :--------------------: | :--------------: |
|      O(n log n)       |      O(n log n)      |         O(n^2)         |     O(log n)     |

기준 item을 정렬하고 왼쪽과 오른쪽으로 나누므로  
나누는 과정의 time complexity는 log 2 n 이다  
이 후 비교하는 과정의 time complexity는 n 이므로
전체 time complexity는 n log n 이다

하지만 최악의 경우는 나누는 과정의 time complexity가 n인 경우다
예를 들어 `[1,2,3,4,6,5]` 라는 data set의 경우  
기준 item이 가장 작은 경우가 반복 되어 n 번 나누게 된다  
이러한 최악의 경우 time complexity 는 n^2이 된다

space complexity 는 pivotIndex 를 저장하기위해  
pivotIndex의 개수 즉, log n 만큼이 필요하다
