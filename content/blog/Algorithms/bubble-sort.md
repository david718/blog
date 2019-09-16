---
title: bubble sort
date: 2019-08-28 10:08:58
category: Algorithms
---

bubble sort는 연속한 두 item의 크기 비교후  
작은 것을 앞으로 보내는 swap을 통해 정렬하는 것이다

이렇게 기본적으로는 모든 item의 크기를 비교하기 때문에  
불필요한 비교연산이 발생한다  
이런 불필요한 비교연산을 제거하며, 즉 덜 비교하며 code가 발전한다  
code의 발전 과정은 아래와 같다

1. 기본 - 모든 item 비교
2. 범위수정 - 정렬되지 않은 item 비교
3. swap check - swap이 일어나는 array만 비교

위 과정을 보면 비교연산의 대상(item)이 줄어드는 것을 알 수 있다  
그 결과 최종적으로 필요한 비교연산만 수행하여 정렬할 수 있게 된다

각 과정에 대해서 이제부터 자세하게 설명하겠다

##1. bubble sort(기본)
bubble sort 는 연속한 두 item을 비교하여 정렬하는 방법이다  
기본적으로 아래의 과정을 생각해볼 수 있다

1. array의 item을 하나씩 돌며 현재 item과 다음 item 크기를 비교한다
2. 다음 item이 작은 경우, 현재 item과 swap한다
3. array의 길이만큼 돌면서 전체 item을 비교한다

위 과정을 아래와 같이 코드로 구현할 수 있다

###bubble sort(기본)

```js{2,3}
function bubbleSort(arr) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr.length; j++) {
      if (arr[j] > arr[j + 1]) {
        let change = arr[j]
        arr[j] = arr[j + 1]
        arr[j + 1] = change
      }
    }
  }
  return arr
}
```

###기본 bubble sort의 한계

위 코드는 하이라이팅된 부분에서 볼 수 있듯이 **array 길이**가 범위이므로
**array 길이의 제곱**만큼 `비교연산(if문)`이 실행된다
이러한 **한계**를 극복하기 위해 비교하는 범위를 수정해야 한다

##2. bubble sort(범위 수정)
위 코드의 범위는 array의 길이만큼이므로  
정렬된 마지막 item까지 계속 비교하고 있다  
이는 불필요한 비교연산이므로  
정렬된 마지막 item은 비교연산 범위에서 뺄 수 있게 범위를 수정하자

```js{2, 3}
function bubbleSort(arr) {
  for (let i = arr.length; i > 0; i--) {
    for (let j = 0; j < i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        let change = arr[j]
        arr[j] = arr[j + 1]
        arr[j + 1] = change
      }
    }
  }
  return arr
}
```

위 코드를 보면 **범위**가 바뀐 것을 확인할 수 있다
i의 범위는 `array 길이`에서 시작해서 `0`까지이다
j의 범위는 `0`에서 시작해서 `i - 1` 까지이다

마지막 item(i번째)은 비교할 필요가 없다
비교가 끝난 후 정렬된 item이기 때문이다
따라서 `i - 1`까지 비교하여 불필요한 비교연산을 하지 않는다

###범위 수정 bubble sort의 한계

`[6,1,2,3,4,5]` 와 같이 거의 정렬이 되어있는 array가 있다  
이때 범위 수정 bubble sort를 통해 정렬하면  
처음 비교할 때 `[1,2,3,4,5,6]` 로 정렬이 완료되었음에도 불구하고  
**계속 비교연산이 실행된다**

즉, 범위 수정 bubble sort의 한계는 거의 정렬이 되어있는 array에서 발생한다

> 정렬이 완료되었음에도 불필요한 비교연산이 실행된다

불필요한 비교연산이 발생하는 이유는  
**swap이 일어나지 않을 때**에도 비교하기 때문이다

그럼 어떻게 swap이 일어날 때만 비교하도록 코드를 짤 수 있을까?

##3. bubble sort(swap check)
swap이 발생하지 않을 때는 반복을 멈출 수 있게 코드를 짜야한다

```js{2, 3}
function bubbleSort(arr) {
  let noSwap
  for (let i = arr.length; i > 0; i--) {
    noSwap = true
    for (let j = 0; j < i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        let change = arr[j]
        arr[j] = arr[j + 1]
        arr[j + 1] = change
        noSwap = false
      }
    }
    if (noSwap) break
  }
  return arr
}
```

위 코드처럼 noSwap 변수를 활용하여  
swap이 발생할때는 noSwap에 false를 할당한다  
noSwap은 기본적으로 true이므로 break하여 반복문을 멈춘다

이로써 정렬이 완료된 배열 즉, noSwap일때는 비교하지 않는다

##bubble sort - time complexity
for문이 2번 돌아가므로  
time complexity의 average 값은 `O(n^2)`이다
