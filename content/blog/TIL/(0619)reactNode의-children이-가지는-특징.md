---
title: (0619)reactNode의 children이 가지는 특징
date: 2019-06-24 14:06:25
category: TIL
---

---
title: [TIL - 0619]react node의 children이 가지는 특징
date: 2019-06-19 21:06:93
category: TIL
---

```js
<Layout>
  <Contents />
</Layout>
```

위와 같은 코드에서 Layout의 children 은 Contents이다.
  
하지만 아래와 같이  

```js
<Layout>
  <Contents1 />
  <Contents2 />
</Layout>
```

2개 이상의 component를 감싸게 되면  
Layout의 children은 Contents가 아니라  
`[<Contents1 />, <Contents2 />]`이 된다.  
  
따라서 2개 이상의 component를 감싼 Layout에서  
`<Contents1 />`을 선택하고 싶을 때는  
반드시 children[0] 이런식으로 item을 지정해주어야 한다.
