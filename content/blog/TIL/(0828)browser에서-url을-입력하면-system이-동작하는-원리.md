---
title: (0828)browser에서 url을 입력하면 system이 동작하는 원리
date: 2019-08-28 15:08:34
category: TIL
---

1. kernal이 curl 을 실행한다
2. curl로 요청을한다 url 입력
3. shared object 필요한 애들을 불러서 확인한다
4. memory에 작업을 올린다
5. 드라이버와 socket을 뚫는다(랜카드와 연결되어있는 socket)(드라이버들 중에서 kernal이 접근 가능한 랜카드에 해당하는 드라이버와 연결)
6. connect (c언어 function 즉 커널의 라이브러리)
7. ssl 활용해서 진짜 구글에서 온 response인지 확인
8. http header를 만들어서 해당 socket 에 쓴다(랜카드로 보낸다)
9. 랜카드에서부터는 OSI 7 layer를 타고 google server로 http header가 전달

##랜카드가 동작하는 과정

- 드라이버: 하드웨어를 조작할 수 있는 API
- 펌웨어: 하드웨어의 소프트웨어, 드라이버가 펌웨어를 사용한다
- 하드웨어: 기계(펌웨어가 명령한 대로 동작한다) -> 랜카드
