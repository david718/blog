---
title: (1003)HTTP cookie
date: 2019-10-03 14:10:64
category: TIL
---

http cookie(웹 쿠키, 브라우저 쿠키)는  
server가 client에게 만들어주는 작은 data 조각이다  
client가 server에 재요청시 data 조각을 함께 전송한다  
그 결과, server는 client를 authenticate 하게 된다  
즉, client 개개인에 맞춰 다른 response를 보낼 수 있게 된다

이러한 cookie의 활용과정과 목적 3가지에 대해 살펴보자
먼저 cookie의 활용 과정이다

## cookie의 활용 과정

cookie는 아래 과정을 거쳐 활용된다

1. server 가 client에게 cookie를 저장한다
2. client에서 cookie를 확인할 수 있다
3. client는 request를 보낼 때 cookie를 함께 보낸다
4. server에서 client가 보낸 cookie를 접근하여 활용한다

### 1. server가 client에게 cookie 저장하기

client가 보낸 request를 받아서  
request에 대한 response의 header에 cookie를 저장한다

```js
http
  .createServer((req, res) => {
    res.writeHead(200, {
      'Set-Cookie': ['yummy_cookie=choco', 'tasty_cookie=strawberry'],
    })
    res.end('cookie')
  })
  .listen(3000)
```

위 서버를 보면 3000 port 에서 request를 받고 있다  
response로 header에 `Set-Cookie`라는 key를 주고  
value값으로 cookie를 client에게 저장하고 있다

### 2. client에서 저장된 cookie 확인하기

일반적으로 client는 브라우저인 경우가 많다  
크롬 브라우저에서는 개발자도구에서 network 탭을 확인해보면  
response headers에 `Set-Coookie`라는 key값으로  
server로부터 cookie를 전달받은 것을 확인할 수 있다

### 3. client에서 server로 cookie 보내기

cookie를 받은 client는 이 후에 request를 보낼 때마다  
자동으로 header에 cookie가 포함되게 된다  
이는 request headers에서 확인할 수 있다

또한 Application tap에서 해당 client가 가지고있는  
모든 cookie를 확인할 수 있다

### 4. server에서 client가 보낸 cookie 확인하기

client에 저장되어있던 cookie는 server가  
request.headers.cookie 를 통해 접근할 수 있다

접근하면 단일 문자열로 cookie가 모두 합쳐져 있다  
이러한 cookie 값을 parsing 하는 module을 활용하면  
cookie를 객체형태로 만들어 활용할 수 있다

cookie라는 module을 활용한 예시 코드가 아래있다

```js
http
  .createServer((req, res) => {
    let cookies = {}
    if (req.headers.cookie !== undefined) {
      cookies = cookie.parse(req.headers.cookie)
    }
    console.log(cookies)
    res.writeHead(200, {
      'Set-Cookie': ['yummy_cookie=choco', 'tasty_cookie=strawberry'],
    })
    res.end('cookie')
  })
  .listen(3000)
```

위 코드를 보면 먼저 cookies라는 빈 객체를 만들었다  
만약 reqeust의 headers.cookie 값이 undefined가 아니면  
cookies에 request.headers.cookie 를 parsing한  
cookie.parse의 return 값을 저장한다  
cookies에는 객체형태로 cookie들이 저장된다

## cookie의 목적 3가지

이러한 cookie는 3가지 목적을 가진다

- session management
- personalization
- tracking

이러한 목적에 따라 cookie를 활용할 수 있다

### session management(ex - sessionid 라는 cookie)

server는 login을 한 client에게 sessionid를 쿠키로 저장한다  
이는 sessionid(cookie)를 통해 client의 login 여부를 확인하여  
다시 login 하지 않아도 login 상태를 유지하기 위함이다

## cookie의 종류

- permanent cookie
- session cookie

web browser 가 종료된 후에도 저장이 계속되는지  
여부에 따라 2종류로 나누어진다

### permanent cookie

permanent cookie 를 만드는 방법은 아래와 같다

```js
http
  .createServer((req, res) => {
    let cookies = {}
    if (req.headers.cookie !== undefined) {
      cookies = cookie.parse(req.headers.cookie)
    }
    console.log(cookies)
    res.writeHead(200, {
      'Set-Cookie': [`permanent=cookie; Max-Age=${60}`],
    })
    res.end('cookie')
  })
  .listen(3000)
```

위처럼 `Max-Age` 라는 값을 주고 = 으로 초단위 숫자로 할당한다  
그럼 해당 초만큼 cookie가 permanent 즉, 유지 된다

### session cookie

`Max-Age` option 을 설정하지 않으면 session cookie이다  
이 cookie는 browser가 종료되면 삭제된다

## cookie의 option 들

다양한 option 을 활용해서 cookie의 기능을 추가할 수 있다

### Max-Age

위 `permanent=cookie; Max-Age=${60}` 에서와 같이  
`Max-Age` 값을 `Max-Age=60` 와 같이 string으로 주면  
해당 cookie는 60초 후에 삭제 된다
즉, client가 저장한 cookie의 수명을 지정할 수 있다

### Secure

`Secure` option 은 아래와 같이 줄 수 있다  
`example=cookie; Secure` 처럼 쓰면 된다  
그 결과 `example=cookie` 이라는 cookie는  
HTTPS 를 활용한 통신에서만 저장하는 cookie가 된다

### HttpOnly

Http를 활용해서 통신할 때만 cookie에 접근할 수 있도록  
cookie에 `HttpOnly` option을 추가할 수 있다

browser에서 javascript로 `document.cookie`를 통해서  
cookie값에 접근할 수 있는데 이때 cookie 탈취의 위험이 있다
`HttpOnly`를 추가하면 javascript로 cookie에 접근이 불가능해진다  
그 결과 cookie 탈취의 위험을 방지할 수 있다

### Path

특정 directory 에서만 cookie를 활성화하고 싶을 때  
해당 directory 를 정하는 option이 path이다

`example=cookie Path=/cookie` 로 path 값을 할당할 수 있다  
그 결과 `localhost:3000/cookie` 에 접속하면  
`example=cookie`라는 cookie가 있는 것을 확인할 수 있다  
 만약 `localhost:3000/` 로 이동하면 해당 cookie는 없다

### Domain

마찬가지로 특정 domain 에서만 활성화 되는 cookie를 만들 수 있다  
`example=cookie Domain=o2.org` 로 Domain 값을 할당할 수 있다  
그 결과 `o2.org`에 접속해야만 cookie를 확인할 수 있다
