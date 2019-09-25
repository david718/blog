---
title: (0913)media query(생활코딩 강의 정리)
date: 2019-09-13 14:09:95
category: TIL
---

media 즉, 화면을 보여주는 다양한 기기에 따라서  
다른 화면을 보여줄 수 있도록 도와주는 query를 말한다

주의해야할 점은 cascading의 원칙을 지켜야 한다

cascading 원칙에 따르면  
더 작은 범위에 적용되는 조건(ex. 더 작은 화면크기)이
나중에 오도록 css 를 작성해야 한다

```html
<style>
  @media (max-width: 600px) {
    body {
      background-color: green;
    }
  }
  @media (max-width: 500px) {
    body {
      background-color: red;
    }
  }
</style>
```

위 코드를 보면  
화면 크기가  
최대 600까지는 green
최대 500까지는 red
이 되는 것을 알 수 있다

즉 화면크기가 작아지다가 600이하가 되면 초록색이고  
500이하가 되면 빨간색으로 바뀐다

하지만 아래 코드를 보자

```html
<style>
  @media (max-width: 500px) {
    body {
      background-color: red;
    }
  }
  @media (max-width: 600px) {
    body {
      background-color: green;
    }
  }
</style>
```

화면크기가 작아지다가 600이하가 되면 초록색이 된다  
하지만 500이하가 되더라도 빨간색이 아닌 초록색이 된다

> 왜냐하면 cascading 원칙에 따라 나중에 작성된 css가 적용되기 때문이다

즉, 600이하는 500이하도 포함한다  
이러한 조건에 따라 css를 작성했기 때문에  
600이하에서도 green이 되고  
500이하에서도 600이하의 조건에 따라 green이 된다

500이하 조건에 따라 red 되었다가  
나중에 작성된 600이하 조건에 따라 green이 되는 것이다

600이하에서는 green  
500이하에서는 red가 되게 하려면  
red가 되는 조건을 더 나중에 작성해야 한다

```html
<style>
  @media (max-width: 600px) {
    body {
      background-color: green;
    }
  }
  @media (max-width: 500px) {
    body {
      background-color: red;
    }
  }
</style>
```

위 코드처럼 작성하는 경우  
600이하일때는 마지막 500이하의 조건인 red가 적용되지 않고  
500이하일때는 마지막 500이하 조건까지 해석하여 red가 된다

이 점을 주의하여 media query를 작성해야 한다
