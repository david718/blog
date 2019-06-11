---
title: Http의 정의와 구성요소, 활용
date: 2019-06-11 19:06:89
category: Developments
---

http는 hyper text transfer protocol이다.  
즉, HTML문서같은 data를 주고 받을 때 지켜야 하는 규약이다.  
data를 주고 받는 주체는 server와 client이다.  
아래 그림을 통해 구체적으로 살펴보자.

![](./images/httpimage.png)

위 그림에서 왼쪽 Web documnet 페이지는 다양한 data로 구성되어 있다.  
png, jpg등의 그림파일, html, css, 비디오 data까지.  
이러한 data는 위 그림의 가운데 Internet을 통해 주고 받는다.  
server(그림 Ads server, Video Server, Web server)는 data를 주고  
client(Web document)는 data를 받아서 페이지를 구성한다.  
  
이렇게 data를 주고 받을 때는 지켜야 하는 규약이 있다.  
그 규약이 HTTP이다.



##References

- https://developer.mozilla.org/ko/docs/Web/HTTP/Overview