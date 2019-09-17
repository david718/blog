---
title: bubble VS selection VS insertion sorting algorithms
date: 2019-09-17 10:09:03
category: Algorithms
---

bubble sort 와 insertion sort, selection sort 는 각각 장단점이 있다

|   Algorithm    | Time Complexity(Best) | Time Complexity(Ave) | Time Complexity(Worst) | Space Complexity |
| :------------: | :-------------------: | :------------------: | :--------------------: | :--------------: |
|  Bubble Sort   |         O(n)          |        O(n2)         |         O(n2)          |       O(1)       |
| Insertion Sort |         O(n)          |        O(n2)         |         O(n2)          |       O(1)       |
| Selection Sort |         O(n2)         |        O(n2)         |         O(n2)          |       O(1)       |

## 각 sorting algoritm의 특징

- bubble sort: 바로 다음 item 과 크기 비교한다. 작은 걸 앞으로 보낸다
- insertion sort: 정렬된 item들을 늘려가는 방식. 이전 item들 중에 정렬된 자리로 현재 item을 보낸다(이미 정렬되어 있으면 비교 끝)
- selection sort: 현재 item이 다음 item들 중에서 가장 작은 item 인지 확인한다. 더 작은 item이 있다면 자리를 바꿔 정렬한다

이러한 특징들을 가지고있기 때문에 각각 효율적으로 적용되는 data set이 다르다

## selection sort 한계와 bubble, insertion sort의 강점

차이는 거의 정렬된 data set의 경우 나타난다

거의 정렬된 data set의 경우 `selection sort`는 **비효율적**이다  
거의 정렬되었다 하더라도 모든 item을 비교해야하기 때문이다

반면 `bubble sort`, `insertion sort` 는  
거의 정렬된 data set의 경우 **효율적**이다  
정렬된 item은 비교하지 않고 정렬되지 않은 item만 비교하기 때문이다
