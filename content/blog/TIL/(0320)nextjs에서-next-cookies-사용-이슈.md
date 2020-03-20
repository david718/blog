---
title: (0320)nextjs에서 next-cookies 사용 이슈
date: 2020-03-20 13:03:24
category: TIL
---

getInitialProps 에서 ctx 인자로 가져왔을 때  
ctx.req 는 server 에서 실행될 때만 있다
browser 즉, client 에서 실행될 때는 ctx.req가 없다

근데 어떻게 next-cookies 에서 가져온 cookies(ctx)는  
client 에서 실행될 때도 cookie 값을 return 하는 거지?

ctx.req 가 없는데 어떻게 ctx.req.headers.cookie 를 읽지?  
나중에 이유 제대로 파악해보자
