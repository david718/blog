---
title: merge sort
date: 2019-09-17 11:09:20
category: Algorithms
---

merge sort는 2가지 사실에 기반한다

- 정렬된 array끼리 sorting하여 merge하는 것은 정렬되지 않은 array들을 merge하는 것보다 빠르다
- item이 0 혹은 1개인 array는 sort 되어있다

위 2가지 사실에 기반하여 merge sort가 진행된다  
이때 먼저 쪼개고, 그 후 merge할 때, 동시에 sort 한다

쪼개는 것 이전에 먼저 merge 하는 function 부터 작성해보자
그 후 쪼개는 logic을 작성할 것이다

## merging arrays pseudocode

1. 빈 array를 만든 후, 각 array에서 가장 작은 item을 순서대로 담는다
2. 두 array의 모든 item을 살펴볼때까지 반복한다
   - 만약 첫번째 array의 item이 두번째 array의 item보다 작다면 더 작은 item을 빈 array에 담고 다음으로 넘어간다
   - 반대라면 반대를 넣고 2번째 array의 다음 item을 비교한다
3. 만약 한 array의 비교가 모두 끝났다면 남은 value들을 모두 합친다

## merging arrays code

```js
function merge(arr1, arr2) {
  let results = []
  let i = 0
  let j = 0

  while (i < arr1.length && j < arr2.length) {
    if (arr2[j] > arr1[i]) {
      results.push(arr1[i])
      i++
    } else {
      results.push(arr2[j])
      j++
    }
  }
  if (i === arr1.length) {
    results = results.concat(arr2.slice(j))
  } else if (j === arr2.length) {
    results = results.concat(arr1.slice(i))
  }

  return results
}
```

## mergeSort pseudocode

merge 하는 function이 완성된 후 이제 쪼개는 logic을 작성할 차례다  
mergeSort는 아래와 같은 pseudocode를 가진다

1. item이 1개일때까지 array를 split 한다
2. 해당 array끼리 비교하여 sorting한 후 merge한다
3. item이 2개가 된 정렬된 array끼리 비교하여 sorting한 후 merge한다
4. item이 4개가 된 정렬된 array끼리 비교하여 sorting한 후 merge한다
5. 모든 item이 merge 될 때까지 정렬된 array끼리 비교하여 sorting한 후 merge한다

즉 merge sort란 sort된 array를 merge하여 전체 array를 sort한다  
여기서 쪼개는 logic은 recursion을 활용한다

## mergeSort code

```js
function mergeSort(arr) {
  if (arr.length <= 1) return arr

  let cutIndex = Math.floor(arr.length / 2)

  return merge(
    mergeSort(arr.slice(0, cutIndex)),
    mergeSort(arr.slice(cutIndex))
  )
}
```

recursion을 활용하는 방법은 위와 같다

우선 base case 를 작성한다  
mergeSort 의 base case는  
arr.length 가 1보다 같거나 작은 경우이다  
item이 0 혹은 1개면 이미 정렬된 array이기 때문이다

그리고 return해야 하는 값은 merge된 array이다  
따라서 merge function을 호출한 결과값을 return해야 한다

이때 merge function에게 arguments가 중요하다

> 다시 mergeSort를 호출한 결과값이 arguments가 된다(이게 recursion의 활용)

base case 즉, item이 0 혹은 1개인 array를 return 할 때까지  
slice로 array를 쪼개서 mergeSort의 인자로 준다

끝까지 쪼개어진 array  
즉, item이 0 혹은 1개가 된 array는 return 되고  
return 된 각각 array 2개는  
merge function에 의해 정렬되어 merge 된다

그렇게 끝까지 쪼개어진 후 다시 정렬되며 merge되는 방식으로  
전체 array가 sort 된다

## Big O of mergeSort

| Time Complexity(Best) | Time Complexity(Ave) | Time Complexity(Worst) | Space Complexity |
| :-------------------: | :------------------: | :--------------------: | :--------------: |
|      O(n log n)       |      O(n log n)      |       O(n log n)       |       O(n)       |

### Time Complexity

best, ave, worst 모두 `O(n log n)`이다  
여기서 n 은 merge function 을 호출할 때  
비교 연산을 arr.length 만큼 수행하기 때문에 발생한다  
log n 은 arr를 반으로 쪼개는 함수가 log 2 n 만큼 실행되기 때문이다  
(n을 계속 2로 나누어서 1이 될때까지 몇번 나누느냐가 곧 log 2 n 임)
log 2 n 이지만 Big O 로 표기하면 log n 이 된다

따라서 최종적인 Time Complexity는 `n * log n` 이므로 `O(n log n)` 이 된다

### space complexity

results 라는 array를 변수에 할당하였으므로  
최종적인 arr의 길이만큼 즉, n 만큼 공간이 필요하다

따라서 Space Complexity는 `O(n)`이다
