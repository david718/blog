---
title: (0826)HTTP
date: 2019-08-29 09:08:53
category: TIL
---

http.createServer() 로 server를 만든 후  
request와 response를 처리할 수 있게 되었다

이 후 client와 server의 통신 과정을 설명하면

1. 브라우저에 url을 입력
2. 브라우저가 DNS server로 가서 domain name에 해당하는 ip주소 받기
3. 해당 ip주소와 일치하는 server로 http request 전달(TCP/IP 4 layer)
   1. Application layer - http request 전달
   2. Transport layer - 통신 노드 연결
   3. Internet layer - 통신 노드 간 packet 전송 및 routing
   4. Network Interface layer - 전기적 신호로 변환하여 실제 전달
4. server에서는 reqeust.url에 따라 routing
5. 해당 url과 일치하는 router function 실행
6. function 실행의 결과(data의 변화, 새로운 data 등)값 얻음
7. 결과값을 브라우저에게 다시 http response 전달(위의 TCP/IP 4 layer 다시 전달)

이때 request와 response가 http 규약을 지킨 packet 으로 구성

##HTTP 구성

- start-line: method, http version, status code
- headers
- empty line
- body

###start-line
보통 http의 개략적인 내용이 간략하게 기술되어 있다

###http headers
header는 request와 response가 차이가 있다

####request header

- GET / HTTP/1.1 : HTTP전송 방법과 프로토콜 버전
- Host: 요청하는 서버 주소
- User-Agent: OS/브라우저 정보
- Accept: 클라이언트 이해 가능한 컨텐츠 타입
- Accept-Language: 클라이언트 인식 언어
- Accept-Encoding: 클라이언트 인코딩 방법
- Connection: 전송 완료후 접속 유지 정보 (keep-alive)
- Upgrade-Insecure-Requests:신호를 보낼때 데이터 암호화 여부
- Content-Type: 클라이언트에게 반환되어야하는 컨텐츠 유형
- Content-Length: 본문크기

####response header

- HTTP/1.1 200 ok : 프로토콜 버전과 응답상태
- Access-Control-Allow-Origin: 서버에 타 사이트의 접근을 제한하는 방침
- Connection: 전송 완료후 접속 유지 정보 (keep-alive)
- Content-Encoding: 미디어 타입을 압축한 방법
- Date: 헤더가 만들어진 시간
- ETag: 버전의 리소스를 식별하는 식별자
- Keep-Alive: 연결에대한 타임아웃과 요청 최대 개수 정보
- Last-Modified: 웹 시간을 가지고 있다 수정되었을때만 데이터 변경 ( 캐시연관 )
- Server: 웹서버로 사용되는 프로그램 이름
- Set-Cookie: 쿠키 정보
- Transfer-Encoding: 인코딩 형식 지정
- X-Frame-Options: frame/iframe/object 허용 여부

####Keep-Alive
Keep-Alive 란 항목은 HTTP/1.1 부터 지원하게 된 항목
response의 body 내용이 전부 download 된 후  
순서에 따라 JS, CSS, Image등의 파일들을 다시 다운받아야 함

이때 앞서 이루어졌던 연결작업을  
모든 파일들을 다운받을 때마다 재반복하려면 시간이 낭비된다

이러한 시간 낭비를 해결하기 위해 Keep-Alive가 등장
접속을 유지하여 연결작업을 재반복할 필요가 없어짐

###body
보통 HTML 문서정보, user data, 수정되어야 하는 data 등, data가 포함되어 있다

##references

- https://www.hostingadvice.com/blog/tcpip-make-internet-work/
- https://flaviocopes.com/http-request/
- https://developer.mozilla.org/ko/docs/Web/HTTP/Overview#HTTP%EC%99%80_%EC%97%B0%EA%B2%B0
- https://www.freeccnastudyguide.com/study-guides/ccna/ch1/1-4-tcpip-model/
-
