---
title: (0904)express의 third-party library
date: 2019-09-10 16:09:24
category: TIL
---

##third-party library란?

express는 server를 구성하는 framework으로  
타인이 만든 library를 가져와서 사용할 수 있다  
이를 third-party middleware라고도 하는데 이는  
express팀이 아닌 제3자가 제작한 middleware를 총칭한다

##middleware 란?
express에서 request를 받았을 때 실행하는 function을 총칭한다
response를 주기 전에 거치는 모든 로직(function)이라고 볼 수 있다

##express에서 middleware를 사용하는 것
보통 url을 routing해서 각 url마다 해당하는 logic을 처리하게 되는데  
이때 routing 하기 전에 middleware 로직을 거치게 된다

즉, 각 routing에서 중복해서 발생하는 작업을 제거할 수 있다  
미리 모아서 중복 작업을 실행한 후에 routing으로 뿌려줄 수 있기 때문이다.

##application-level middleware 작성법

```js
const express = require('express')
const app = express()

app.use((req, res, next) => {
  fs.readdir('./data', function(error, filelist) {
    req.list = filelist
    next()
  })
})
```

위와 같이 router 별로 중복되는 code를 추출하여  
request에 list 변수를 선언하고 할당할 수 있다

그 결과 다른 router에서는 req.list로 해당 변수에 접근할 수 있다

##application-level middleware 실행순서

```js
app.use((req, res, next) => {
  //반드시 next()를 호출해주어야만 다음 middleware가 실행된다
  //next()를 호출하지 않으면 더이상 다음 middleware가 실행되지 않는다
})
```

next()를 활용하여 middleware의 실행순서를 통제할 수 있다

##middleware로, third-party library중 하나인 body-parser를 사용하는 예시

body-parser란 form 형식 혹은 json 형식으로 전송된 data를  
쉽게 parsing 해주는 도움을 주는 express에서 사용 가능한  
third-party library이다

###body-parser 사용하기 전 코드

```js{10,11,12,13}
const express = require('express')
const bodyParser = require('body-parser')
const qs = require('querystring')

const app = express()

//urlencoded 는 form data 전송시 해당
app.use(bodyParser.urlencoded({ extended: false }))

app.post('/create_process', (req, res) => {
  var body = ''
  req.on('data', function(data) {
    body = body + data
  })
  req.on('end', function() {
    var post = qs.parse(body)
    var title = post.title
    var description = post.description
    fs.writeFile(`data/${title}`, description, 'utf8', function(err) {
      res.writeHead(302, { Location: `page/${title}` })
      res.end()
    })
  })
})
```

위 하이라이트된 부분에서 볼 수 있듯이
request에서 보낸 form data를 받는 과정은 다음과 같다.

1. body를 빈 string 을 할당하여 선언한다
2. request가 data를 받는 동안 body에 data를 더하여 할당한다
3. 모은 body를 querystring을 활용하여 parsing 한다

이러한 3단계 과정을 1단계로 축약시켜주는 것이 body-parser이다

###body-parser 사용하여 data를 request.body에 할당한 코드

```js
app.post('/create_process', (req, res) => {
  var post = req.body
  var title = post.title
  var description = post.description
  fs.writeFile(`data/${title}`, description, 'utf8', function(err) {
    res.writeHead(302, { Location: `page/${title}` })
    res.end()
  })
})
```

post에 `req.body`를 할당한 것을 확인할 수 있다
즉, body에 모은 data를 querystring으로 parsing하는 과정을  
body-parser는 모두 처리하여 request에 body에 할당하였다

request가 form action을 통해 전송한 data를  
parsing하여 body에 할당한 것이다

따라서 post에 할당하여 활용하면 끝이다
