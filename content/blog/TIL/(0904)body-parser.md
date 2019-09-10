---
title: (0904)body-parser
date: 2019-09-10 16:09:24
category: TIL
---

body-parser란 form 형식 혹은 json 형식으로 전송된 data를  
쉽게 parsing 해주는 도움을 주는 express에서 사용 가능한  
third-party library이다

##body-parser 사용하기 전 코드

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

##body-parser 사용하여 data를 request.body에 할당한 코드

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
