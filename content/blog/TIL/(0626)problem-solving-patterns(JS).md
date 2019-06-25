---
title: (0625)problem solving patterns(JS)
date: 2019-06-25 16:06:12
category: TIL
---

##Frequency counters(빈도수 세기)
object를 사용하여 빈도수를 세는 문제 해결 전략이다  
  
세어야 할 item을 key, 빈도수를 value로 할당한다  
JS에서 기본적인 counting 문제의 해결 전략

##multiple pointers
item을 가리키는 pointer를 여러개 설정하자  
이 pointer 들을 활용하여 문제를 해결하는 전략이다  
  
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

###정렬된 array에서 유니크한 item 개수 구하기
위 문제는 정렬된 item에서 중복 없이 유니크한 item 수를 구하는 것이다  
이 문제또한 multiple pointers로 해결할 수 있다  
  
(pseudo code)
시작 pointer와 비교 pointer 2가지 pointer를 움직이면서  
값을 비교하고 같으면 비교 pointer를 움직이고 다르면 시작 pointer를 움직인다  

```js
function countUniqItem(arr) {
  if(arr.length === 0) {
    return 0;
  } else if(arr.length === 1) {
    return 1;
  }
  let i = 0;
  let j = 0;
  for(j = 1; j < arr.length; j++) {
    if(arr[i] !== arr[j]) {
      i++;
      arr[i] = arr[j];
    }
  }
  return i + 1;
}
```

위와 같이 i 와 j pointer 를 움직여서 답을 구할 수 있다

##sliding window pattern
특정 범위의 item을 포함하는 창문(sub set)을 만든다  
창문(sub set)은 data set안에서 이동하며 탐색한다  
이 창문의 특성을 활용하여 문제를 해결하는 전략이다

아래 문제를 sliding window pattern 으로 풀 수 있다
  
array에서 4개의 연속된 item의 합 중 가장 큰 값을 찾아라

```js{12}
function maxSubarraySum (arr, num) {
  if(arr.length < num) return null;

  let maxSum = 0;
  let tempSum = 0;

  for(let i = 0; i < num; i++) {
    maxSum += arr[i];
  }
  tempSum = maxSum;
  for(let i = num; i < arr.length; i++) {
    tempSum = tempSum - arr[i - num] + arr[i];
    maxSum = Math.max(maxSum, tempSum);
  }
  return maxSum;
}
```

subArray의 모든 값을 계속 다시 더할 필요 없이  
원래 알고 있는 합 값에서 맨처음 값(`arr[i-num]`)을 빼고  
맨마지막 값(`arr[i]`)을 더하면 새로 비교할 합 값이 된다  
그렇게 새로 비교할 합 값으로 계속 비교하면서 최대 합 값을 찾는다  

##Divide and Conquer
data set을 더 작게 쪼갠 후  
작은 data set에 같은 과정을 반복하여 문제를 해결하는 방법  
창문(sub set)과 다르게  
범위에 상관없이 특징에 따라 sub set을 쪼갠다  
 