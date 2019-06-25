---
title: (0613)webpack-dev-server & react-hot-loader
date: 2019-06-13 15:06:59
category: TIL
---

##webpack-dev-server
webpack.config.js 파일을 읽어서 빌드(번들링)를 한 후  
localhost에서 접속하여 빌드한 파일을 확인할 수 있도록  
server에 대한 localhost의 port를 열어준다.
 
##react-hot-loader
react에서 DOM 전체를 새로고침 하는 것이 아니라  
DOM의 특정 element가 변화하면 그 변화한 부분만 업데이트한다.  
  
먼저 hot을 불러와야 한다.  
`react-hot-loader/root`에서 가져올 수 있다.  
  
최상위 component, 즉 보통은 App component를  
`hot(<App />)` 이런식으로 감싸주어 사용한다.  
  
그 결과 `<App />`의 특정 element가 변화하면  
페이지 전체가 다시 rendering 되는 것이 아니라  
그 변화한 부분만 반영하여 업데이트가 된다.  

##webpack-dev-server --hot
react-hot-loader 를 설치한 후 webpack-dev-server를 켤 때  
--hot 옵션을 붙여서 명령어를 입력하게되면  
server는 DOM 업데이트가 발생할 때마다 재실행이 된다.  
