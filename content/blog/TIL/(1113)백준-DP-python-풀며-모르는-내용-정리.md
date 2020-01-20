---
title: (1113)백준 DP python 풀며 모르는 내용 정리
date: 2019-11-13 14:01:91
category: TIL
---

## 11053 가장 긴 증가하는 부분 수열

```py
import sys
n = int(sys.stdin.readline())
nums = list(map(int, sys.stdin.readline().split()))

dp = [0 for i in range(n)]

for i in range(len(nums)):
    for j in range(i):
        if nums[j] < nums[i]:
            dp[i] = max(dp[j], dp[i])

    dp[i] += 1

print(max(dp))
```

### 의도

가장 긴 수열의 길이를 구하는 문제이다  
따라서 DP 문제 임을 유추할 수 있다  
(앞 요소까지 중 최대 길이 값에 자신을 붙여 +1 을 한 값)

### pseudo code

1. 요소 전체를 돌면서 자신과 앞 요소를 비교한다
2. 앞 요소들 중에서 자신보다 작은 값의 요소를 선택한다
   (자신보다 크다면 증가하지 않으므로 제외한다)
3. 자신보다 작은 값의 요소들이 가진 수열의 길이 값 중 최대값을 저장한다
4. 최대값에 자신의 길이(+1)를 더한다
   (자신보다 작은 값이므로 자신을 추가할 수 있다)

### 개선점

2중 포문을 돌기때문에 O(n) = n^2 이다  
한번에 돌면서 체크할 수 있는 방법을 더 고민해 봐야겠다

##
