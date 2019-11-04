---
title: Radix sort
date: 2019-11-04 11:11:09
category: Algorithms
---

Radix sort 는 자리수가 다른 숫자들을 sorting 할 때  
가장 빠른 sorting algorithm 이다

## pseudo code

1. 각 숫자의 1의 자리수에 숫자를 체크한다
2. 0부터 9까지의 bucket 을 만들어 해당 1의 자리 숫자를 bucket 에 담는다
3. bucket 에 담긴 순서대로 1번 정렬된 array를 return 한다
4. 2의 자리수 부터 가장 큰 자리수에 숫자를 체크할때까지 위 과정을 반복한다
5. 마지막으로 정렬된 array를 return 한다

## helper function

- getDigit
- digitCount
- getMaxDigitCount

### getDigit

숫자와 자리수를 받아서  
해당 숫자의 자리수에 있는 수(0 - 9 사이)를 리턴한다

```js
const getDigit = (num, place) => {
  return Math.floor(Math.abs(num) / Math.pow(10, place)) % 10
}
```

### digitCount

숫자를 받아서  
해당 숫자의 자리수(숫자길이)를 리턴한다

```js
const digitCount = num => {
  return num.toString().length
}
```

### getMaxDigitCount

숫자 array를 받아서  
모든 숫자 중 가장 긴 자리수(숫자길이)를 리턴한다

```js
const getMaxDigitCount = nums => {
  const digitCounts = nums.map(item => digitCount(item))
  return digitCounts.reduce((a, b) => Math.max(a, b))
}
```

## Radix sort code

먼저 bucket이 필요하다

```js
let buckets = {
  0: [],
  1: [],
  2: [],
  3: [],
  4: [],
  5: [],
  6: [],
  7: [],
  8: [],
  9: [],
}
```

위 bucket 에 getDigit으로 리턴받은  
해당 자리의 숫자를 체크하여 숫자가 같은 array에 담는다  
그리고 담은 순서대로 array에 넣어 정렬한다  
위 과정을 가장 긴자리의 수까지 체크하며 반복한다

```js
const radixSort = arr => {
  const maxDigitCount = getMaxDigitCount(arr)

  let sortedArr = arr
  for (let i = 0; i <= maxDigitCount; i++) {
    sortedArr.forEach(num => {
      buckets[getDigit(num, i)].push(num)
    })
    sortedArr = []
    for (key in buckets) {
      while (buckets[key].length !== 0) {
        sortedArr.push(buckets[key].shift())
      }
    }
  }

  return sortedArr
}
```

## Big O of Radix Sort

| Time Complexity(Best) | Time Complexity(Ave) | Time Complexity(Worst) | Space Complexity |
| :-------------------: | :------------------: | :--------------------: | :--------------: |
|         O(nk)         |        O(nk)         |         O(nk)          |     O(n + k)     |

- n : array의 길이
- k : 가장 긴 자리의 자리수
