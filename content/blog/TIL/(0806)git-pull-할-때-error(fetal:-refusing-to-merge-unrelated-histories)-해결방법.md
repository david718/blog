---
title: (0806)git pull 할 때 refusing to merge unrelated histories) 해결방법
date: 2019-08-08 18:08:53
category: TIL
---

cli 로

```js
git pull origin master(branch 이름)
```

를 입력했을 때 아래와 같은 에러가 발생한 경우

> fetal: refusing to merge unrelated histories

이를 해결하는 가장 간단한 방법은 옵션을 추가하는 것이다

git pull origin (branch 이름) --allow-unrelated-histories

라는 --allow 옵션을 추가해주면 해결된다!
