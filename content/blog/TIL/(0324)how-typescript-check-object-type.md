---
title: (0324)how typescript check nested object type
date: 2020-03-25 11:03:23
category: TIL
---

typescript 가 nested object type 을 check하는 방법은

> 해당 type(object)의 최하위 key 가 전부 존재하는지이다

1. object type 의 예시로 User 를 선언한다
2. 모든 key를 가지며 type 과 일치하는 nested object 인 David 를 선언한다
3. 모든 key를 갖고 있지만 nested 되지 않은 object 인 John 을 선언한다
4. 둘 다 User type 이 맞다고 선언하는지 확인한다

아래와 같은 User type을 선언했을 때

```ts
type User = {
  name: string
  car: {
    company: string
    number: number
  }
}
```

아래 David 는 User가 맞다

```ts
const david = {
  name: 'david',
  car: {
    company: 'bmw',
    number: '1432',
  },
}
```

그러나 typescript 의 object type check 에 의하면  
아래 John 도 User가 맞다고 말한다

```ts
const john = {
  name: 'john',
  company: 'benz',
  number: '7878',
}
```

object type의 최하위 key를 모두 포함한 object라면  
즉, `name`, `company`, `number`를 포함한 object라면  
typescript는 해당 value(john)이 User type이 맞다고 확인한다

car가 object를 value로 가지지 않더라도  
**즉, nested 된 object 형태가 아니더라도**
david와 john 은 다른 값이지만 같은 User type 이라고 확인한다
