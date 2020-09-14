---
title: web 개발자가 무슨일 하지?
date: 2020-08-07 13:08:13
category: Developments
---

web 개발자가 되려고 공부를 시작할지 고민하는 사람들은  
web 개발자가 무슨 일을 하는지 궁금해한다

아래에서는 web 개발자가 하는 개발에 대한  
전체적인 과정 목록과 각 과정에 대해 자세히 설명하려 한다

1. 웹 개발이란?
2. 웹 개발의 가장 쉬운 예시

## 1. 웹 개발이란?

여기서 웹 개발이란 웹사이트를 보여주기 위해 필요한  
client에서의 App과 server의 개발을 말한다

집 컴퓨터에서 웹사이트(예 naver)에 접속하는 과정을 통해  
client에서의 app 과 server 를 알아보자

1. 집 컴퓨터의 크롬 혹은 인터넷 익스플로어 등 브라우저를 실행한다
2. 브라우저 주소창에 웹 사이트 주소를 입력한 후 엔터
3. 집 컴퓨터가 naver 의 컴퓨터(server)로 request 한다(인터넷 연결 ok일때)
4. naver 의 컴퓨터(server)는 해당하는 파일(HTML, CSS, javascript)을 response 한다
5. 브라우저에서 파일을 받아서 www.naver.com 화면을 그린다(rendering 한다)

그려진 naver 화면(초록색 배경, 검색창 및 버튼, 로그인 버튼 등)이 client의 App이다  
이 App을 개발하는 것이 frontend 개발자의 역할이다

파일을 serve(response)하는 naver 의 컴퓨터(server)가 server다  
이 server를 개발하는 것이 backend 개발자의 역할이다

web 개발자는 이 2가지 역할을 포함한다

추상적인 개념을 쉽게 이해하기 위해 구체적으로 실습해보자

## 2. 웹 개발의 가장 쉬운 예시

메모장을 켜보자

- 실습
  1. 메모장에 아래 코드를 따라쳐보자
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <h1>세젤쉬 웹사이트</h1>
      <p>웹사이트 만들기가 가장 쉬웠어요</p>
    </body>
  </html>
  ```
  2. 메모장 파일 이름을 **test.html 로 저장**해라
  3. 저장한 메모장 파일의 주소를 복사해라
  4. 브라우저를 열어라
  5. 브라우저 주소창에 아까 복사한 메모장 파일 주소를 붙여넣기하고 엔터
  6. 브라우저 화면에 뜬 세젤쉬 웹사이트를 확인

세상에서 제일 쉬운 웹사이트를 만들었다

## 3. 세젤쉬 웹사이트의 동작 원리?

- 우리가 저장한 메모장의 내용은 html 이라는 언어로 쓰여진 코드이다(아무이름.html 로 저장하면 컴퓨터는 html 파일로 인식함)
  - html 언어는 hyper text markup language 로 브라우저가 해석하여 화면을 그려줄 수 있는 정보를 전달하는 언어이다 (html 에 대한 블로그글 비디오 강의 추가)
- 메모장 파일을 저장했다는 것은 우리 컴퓨터에 html 파일이 저장되었다
- 메모장 파일의 주소는 html 이 우리 컴퓨터에 저장된 위치를 말해준다
- 브라우저 주소창에 이 주소를 입력하고 엔터를 치면?
  1. 브라우저는 이 주소에 해당하는 우리 컴퓨터(server)에게 request 를 보낸다
  2. 우리 컴퓨터(server)는 response 로 메모장에 저장된 html 을 보낸다
  3. 브라우저는 html 을 response 를 rendering 한다

## 4. server 와 client

위 세젤쉬 웹사이트의 동작 원리에서  
우리 컴퓨터를 server 라고 부르는데 server 는 뭘까

server 는 request에 해당하는 파일(html)을 response 를 하는 주체이다  
naver 의 서버실에 있는 컴퓨터는 response 를 하기에 server 라고 부를 수 있다

따라서 우리 컴퓨터도 response 를 한다면 server 라고 부를 수 있다  
또한 우리 컴퓨터가 request 를 한다면 client 라고도 부를 수 있다

위 과정에서 알 수 있듯이 주소창에 주소를 입력하면 브라우저는 request 한다  
따라서 request를 하는 브라우저를 일반적으로 client 라고 부른다

즉, server 혹은 client 는 대상이 무엇이냐  
(우리 컴퓨터냐, 네이버 서버실 컴퓨터냐)에 따라 결정되는 것이 아니라  
대상이 하는 **행위(request 혹은 response)에 따라** 결정된다

request 는 요청이고 response 는 응답이다  
우리 컴퓨터가 웹사이트를 그리기 위해서는 html 파일이 필요하다  
이 html 파일을 server에게 request 한다  
server는 request에 해당하는 파일을 response 한다

즉, 대상이 request 를 하면 client, response 를 하면 server이다

- client 는 request
- server 는 response

## 5. 브라우저가 www.naver.com 를 그리는 상세 과정

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
