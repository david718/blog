---
title: Http proxy
date: 2020-05-25 16:05:48
category: Developments
---

http 는 프로토콜 hyper text transfer protocol  
즉 server 와 client 가 web 에서 통신하기 위한 규약

### http 가 적용되는 web의 구성요소

- client : request 를 보내는 객체(ex) browser)
  - HTML, CSS, Javascript, image, video etc 를 조합하여 웹페이지 rendering
- server : HTTP/1.1 과 host header 를 활용, 동일한 ip 주소를 공유
- proxy

client 에서 request 를 보내면 proxy 를 거쳐  
server 에 전달된다  
server 는 response 를 보내고 proxy 를 거쳐  
client 가 받게된다
