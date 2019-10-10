---
title: (0915)axios get with params option
date: 2019-09-15 14:10:92
category: TIL
---

axios 는 비동기 요청을 도와주는 library이다
promise based HTTP client for the browser and node.js

즉, browser와 node.js 에서 server와 통신할 때  
HTTP기반의 promise를 만들어주고 request를 보낼 수 있게 한다

먼저 client에서 get 요청을 보내보자

## axios 활용 client에서 get 요청 보내기

```js
axios
  .get('/videos/remove', {
    params: {
      id: id,
    },
  })
  .then(res => {
    console.log(`${res.data.id} File is deleted`)
  })
  .catch(err => {
    console.error(err)
  })
```

위 코드를 보자. 먼저 `axios.get()`으로 시작한다  
이 get method는 우리가 아는 http의 `GET` method와 유사하다  
get HTTP request를 보내는 것이다  
(인자로는 path와 option이 들어간다)

path로 `/videos/remove`를 주고 있다
해당 url 로 request가 간다
option으로는 params의 값으로 `{ id: id }` object가 들어갔다
즉 path에 query가 추가된 get 요청이다

주의할 점은 key 이름이 params 인데  
추가되는 path 방식은 query이다  
즉 정리하면 최종적으로 요청이 가는 url은  
`/videos/remove?id=${id}`가 되는것이다

## server에서 axios 로 client에서 보낸 get 요청을 받기

```js
router.get('/remove', (req, res) => {
  const { id } = req.query
  log(util.inspect(id))

  fs.unlink(`${uploadPath}/${id}`, err => {
    if (err) throw err
    console.log(`delete ${id}`)
  })

  res.send({
    id,
  })
})
```

이미 express 의 router 까지 설정해 놓은 상태이다
위 코드를 보면 router가 있다  
이 router는 `/videos`에 해당하는 요청을 전부 처리하는 router이다  
따라서 `/remove`는 곧 `/videos/remove`이다

위 클라이언트에서 `/videos/remove`로 보내진 요청을  
처리하는 로직에 해당하는 api이다  
이때 가장먼저 클라이언트의 요청 즉, req에서  
req.query.id 값을 꺼낸다
`const { id } = req.query` 코드가 이에 해당한다  
그리고 `fs.unlink`로 해당 id의 file을 삭제한다

마지막으로 res.send 로 즉, response 요청에 대한 응답으로  
삭제된 file의 id를 보내준다

## axios 활용 주의점 및 마무리

기본적인 axios 활용 예시를 살펴보았다  
client가 요청하고 server 가 응답하는 과정에서  
서로 통신을 성공하기 위해서는  
올바른 endpoint로 요청을 보내고 응답로직을 작성해야 한다
endpoint가 맞지 않으면 client와 server는 서로 통신할 수 없다

이때 params와 query를 구분하는 것이 필요하다
