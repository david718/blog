---
title: (1015)session, express-session
date: 2019-10-15 15:10:74
category: TIL
---

## session 이란?

cookie를 활용해 client마다 맞춤 response를 보낼 수 있게 되었다  
또한 login 정보를 저장하여 login 상태를 유지할 수 있게 되었다  
**이를 위해서는 cookie data를 client에게 저장해야 했다**

session 은 이러한 cookie data를  
client가 아닌 server에 저장한다

client를 확인하기 위한 id 만을  
client에게 cookie로 저장하고  
해당 client를 식별하기 위한 data는  
server에 저장한다

## express-session

express의 middleware로  
session 활용을 돕는 library이다  
기본적으로 server 의 memory 에  
cookie data를 저장한다

### req.session 객체 만들기

```js
app.use(
  session({
    secret: 'keyboard cat',
    resave: false,
    saveUninitialized: true,
  })
)
```

위와 같이 `app.use`로 session을 만든다  
3가지 option이 있다

- secret: session data를 확인하기 위해 필요한 secret key 이다
- resave: session 저장소 data 의 변화 여부에 따라서 저장 여부를 결정하는 옵션
- saveUninitialized: session이 필요하기 전까지는 express-session을 동작하지 않는다

> session 객체의 data 는 memory에 저장된다  
> 따라서 server를 재시작하면 session에 저장된 data가 모두 삭제된다

### req.session 객체 접근하기

```js
app.get('/', (req, res) => {
  req.session
})
```

`req.session`을 통해서 session 객체에 접근 가능하다

## session store

session 의 data 는 memory에 저장되기 때문에  
server를 재시작하면 data 가 모두 삭제된다  
이러한 한계를 극복하기 위해  
data를 memory가 아닌 다른 곳에 저장할 수 있도록  
저장소를 설정하는 옵션이 session store이다

### session-file-store

```js
const session = require('express-session')
const FileStore = require('session-file-store')(session)

app.use(
  session({
    secret: 'keyboard cat',
    resave: false,
    saveUninitialized: true,
    store: new FileStore(),
  })
)
```

위와 같이 store 의 value로  
`session-file-store`에서 session을 넣어 실행한  
return 값을 할당한다

그 결과 해당 session 에 저장되는 data는  
memory가 아닌 store의 value 값에  
즉, FileStore에 저장된다  
(path 의 default 는 session directory 이다)

### request.session.save()

memory에 저장해둔 session 객체 data 를
session store 에 저장할 때까지 시간이 걸린다

따라서 session store에 저장한 후  
다음 로직을 실행하고 싶다면  
아래처럼 `request.session.save()` 의 callback으로  
로직을 전달하면 된다

```js
request.session.save(() => {
  // 다음 로직
})
```
