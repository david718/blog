---
title: (0605)web server 구축
date: 2020-06-05 12:06:47
category: TIL
---

web server 란 client 의 request 에 response를 주는  
application 이다

모든 web server 에는 ip 주소가 있다  
ip 주소는 숫자 4개로 구성되어 있어서 기억하기 쉽지 않다  
기억하기 쉬운 domain 주소를 만들어 ip 주소와 연결할 수 있다  
DNS(domain name server)가 domain 주소와 ip 주소를 연결한다

### DNS

동작 과정은 아래와 같다

1. client 가 browser 주소 창에 domain 주소를 입력한다
2. browser 는 DNS 로 가서 domain 주소에 해당하는 ip 주소를 request 한다
3. DNS 는 request 한 domain 주소에 해당하는 ip 주소를 response 한다
4. browser 는 ip 주소로 reqeust 한다
