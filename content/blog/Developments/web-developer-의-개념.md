---
title: web 개발의 개념
date: 2020-08-07 13:08:13
category: Developments
---

## 1. 웹 개발이란?

여기서 웹 개발이란 웹사이트 개발을 말한다

- 웹 사이트란?

  1. 크롬 혹은 인터넷 익스플로어 등의 브라우저를 실행한다
  2. 브라우저 주소창에 웹 사이트 주소를 입력한다.
  3. www.naver.com 창이 뜬다 검색할 수 있도록
  4. 이 창이 보여주는 화면을 우리는 웹사이트라고 부른다

- 위 과정을 자세하게 설명하면

  1. 우리가 브라우저를 실행해서 주소창에 주소를 입력하면
  2. 브라우저는 server(다른 컴퓨터) 로 request 를 보낸다
     (이 server 는 한국에 네이버 서버실에 있는 컴퓨터이다)
  3. 방에 연결된 인터넷 선 혹은 와이파이로 인터넷 라우터까지 request를 전달한다
  4. 라우터가 네이버 서버실에 있는 컴퓨터까지 선을 타고 request를 전달한다
     (http 형태로 보내지는 request 이다)
  5. server 가 request 를 실행해서 이에 해당하는 response 를 보낸다
  6. 인터넷 선을 타고 집에 있는 컴퓨터에게 response 를 전달한다
  7. 컴퓨터는 브라우저에게 response 를 전달한다  
     (response 도 http 형태이다)  
     (response 는 html, css, javascript 를 포함하고 있다)
  8. 브라우저는 html, css, javascript 를 해석해서 화면에 rendering 한다

## 2. 위 과정의 가장 쉬운 예시

메모장을 켜라

- 실습
  1. 메모장에 아래 코드를 따라쳐라(복사 붙여넣기 ㄴㄴ)
  ```html
  <html>
    <body>
      <h1>세젤쉬 웹사이트</h1>
      <p>웹사이트 만들기가 가장 쉬웠어요</p>
    </body>
  </html>
  ```
  2. 메모장 파일 저장해라
  3. 저장한 메모장 파일의 주소를 복사해라
  4. 브라우저를 열어라
  5. 브라우저 주소창에 아까 복사한 메모장 파일 주소를 붙여넣기하고 엔터
  6. 짜잔!

방금 세상에서 제일 쉬운 웹사이트를 만들었다

## 3. 세젤쉬 웹사이트의 동작 원리?

- 우리가 저장한 메모장의 내용은 html 이라는 언어로 쓰여진 코드이다
  - html 언어는 hyper text markup language 로 브라우저가 해석하여 화면을 그려줄 수 있는 정보를 전달하는 언어이다(html 에 대한 블로그글 비디오 추가해라)
- 메모장 파일을 저장했다는 것은 우리 컴퓨터에 html 파일이 저장되었다
- 메모장 파일의 주소는 html 이 우리 컴퓨터에 저장된 위치를 말해준다
- 브라우저 주소창에 이 주소를 입력하고 엔터를 치면?
  1. 브라우저는 이 주소에 해당하는 우리 컴퓨터(server)에게 request 를 보낸다
  2. 우리 컴퓨터(server)는 response 로 메모장에 저장된 html 을 보낸다
  3. 브라우저는 html 을 포함한 response 를 받아서 해석하여 화면에 rendering 한다

## 4. server 와 client

우리 컴퓨터를 server 라고 부르는데 server 는 뭘까

server 는 serve, 즉 request에 대해 response 를 하는 주체이다  
naver 의 서버실에 있는 컴퓨터도 response 를 한다면 server 라고 부를 수 있다  
따라서 우리 컴퓨터가 response 를 한다면 server 라고 부를 수 있다  
또한 우리 컴퓨터가 request 를 한다면 client 라고 부를 수 있다  
위 과정에서 알 수 있듯이 주소창에 주소를 입력하는 것이 request 이다  
이 request 를 하는 주체가 client 이다  
일반적으로 우리 컴퓨터에서 실행되는 브라우저를 client 라고 부른다

즉, server 혹은 client 는 대상이 무엇이냐  
(우리 컴퓨터냐, 네이버 서버실 컴퓨터냐)에 따라 결정되는 것이 아니라  
대상이 하는 행위(request 혹은 response)에 따라 결정된다
