---
title: (1106)python 백준 IO 문제 풀면서 모르는 내용 정리
date: 2019-11-06 12:01:77
category: TIL
---

## 풀었던 문제 목록

- 1000
- 2558, 2557
- 10950, 10951, 10952, 10953
- 11021, 11022
- 11718, 11719, 11720, 11721
- 2741, 2742, 2739
- 1924
- 8393
- 10818
- 2438, 2439, 2440, 2441, 2442, 2445, 2446
- 2522
- 10991, 10992

### 입력은 input 대신 sys.stdin

```py
import sys

for line in sys.stdin:
    print(line, end='')
```

sys.stdin 이 속도가 더 빠르다

### print 는 끝나면 자동으로 다음줄로 이동

따라서 다음줄로 이동하지 않으려면

```py
print('내용 입력', end='')
```

위와 같이 end 옵션에 값으로 `''` 즉 빈 문자열을 주면 해결

### 11721번 - range 의 세번째 arguments 는 step(숫자 사이 거리)

```py
import sys
str = sys.stdin.readline()

while len(str) >= 10:
    print(str[:10])
    str = str[10:]

print(str, end='')
```

시간은 56ms 로 위 풀이가 아래 풀이(64ms)보다 더 빨랐다  
하지만 range 의 step 을 적용하는 취지에서

```py
import sys
str = sys.stdin.readline()

for i in range(0, len(str), 10):
    print(str[i:i+10])
```

위와 같이 풀어서 마무리했다  
세번째 들어가는 숫자 10이 step 즉 i 사이 간격이다

### 2742번 reversed

reversed 는 python 내장 함수로 reversed 된 객체를 return  
reverse 는 list 의 method 로 list 의 순서를 뒤집어준다

```py
import sys

n = int(sys.stdin.readline())

for i in reversed(range(1, n + 1)):
    print(i)
```

range의 item 숫자를 역순으로 print 한다

### 8393번 연산자 `/` 의 결과는 소수점 1자리로 나온다

```py
import sys
n = int(sys.stdin.readline())
print(n * (n+1) // 2)
```

`//` 연산자 사용하면 몫(정수)을 구할 수 있다

### 10818번 max, min 내장함수

max 와 min 내장함수는 list 를 arguments 로 받는다

```py
import sys
nums = list(map(int, sys.stdin.readline()))
print(f'{min(nums)} {max(nums)}')
```
