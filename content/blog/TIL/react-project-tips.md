---
title: (0608)webpack config 설정방법
date: 2019-06-08 09:06:32
category: TIL
---

##webpack이란
javascript의 모듈을 하나의 파일로 합치는(번들링) 역할  

```js
const path = require('path');

module.exports = {
  name: "wordrelay-setting",
  mode: "development", //실서비스  production
  devtool: "eval",
  resolve: {
    extensions: [".js", ".jsx"]
  },

  entry: {
    app: ["./client"] //다른 파일에서 불러오는 것은
  }, //입력
  module: {
    rules: [{ test: /\.jsx?/, loader: "babel-loader", options: {
      presets: [
        ['@babel/preset-env', {
          targets: {
            browsers: ['last 2 chrome versions'],
          },
          debug: true,
        }],
        '@babel/preset-react',
      ],
      plugins: ['@babel/plugin-proposal-class-properties'],
    }, }]
  },
  plugins: [
    new webpack.LoaderOptionsPlugin({debug: true}),
  ],
  output: {
    path: path.join(__dirname, "dist"),
    filename: "app.js"
  }, //출력
};
```

webpack은 **entry**의 파일을 입력으로 받아서  
**module**의 도움을 받아
**output**의 경로와 파일 이름으로 출력한다.  

- name: webpack 설정파일의 프로젝트 이름
- mode: 개발인지 서비스인지(development or production)
- devtool: 'eval'(빠르게 진행하도록? 잘 모름 더 찾아보자)
- resolve: extensions 라는 옵션에 확장자명 넣어두면 해당 확장자 전부 찾아서 번들링 함
- entry: 하나로 합칠 파일이름(key값)과 합칠 다른 여러 파일들(value array)을 설정
- module: webpack은 기본적으로 js, json만 해석 가능 다른 확장자의 파일을 해석하기 위해 module 사용
  - test: 해석해야 할 다른 확장자들
  - loader: 사용할 module의 이름
  - options: module안에 plugin들의 모음(preset은 plugin 모아둔 것)
    - presets: option에서 적용되는 target을 설정할 수 있다
- plugins: 확장 프로그램이라 생각하자 module 외에 추가로 필요한 프로그램 가져다 쓸 수 있음
- output: 합친 결과물 파일을 저장할 경로와 파일 이름 설정  

###cli 설정 방법

등록 안되어 있는 cli를 실행하는 방법은 2가지가 있다

1. package.json에서 script에 해당 명령어를 설정
2. npx + 해당 명령어(webpack)

##Babel
Babel은 javascript의 버전이 다른 경우  
하나의 버전으로 만들어 주는 역할을 한다.  
webpack과 함께 쓰여 하나의 버전으로 javascript를 만들고  
합칠 수 있게 한다.  

- @babel/core: 가장 기본적인 바벨 파일 최신 문법을 바꿔준다
- @babel/preset-env: 환경에 맞게 해석 가능한 javscript로 바꿔준다
- @babel/preset-react: react(jsx)를 만났을 때 javascript로 바꿔준다
- babel-loader: babel과 webpack을 연결해주는 것

##webpack 실행하여 번들링하는 과정
하나의 js파일로 합치는(bundling) 과정을 아래와 같다.

1. npx webpack 입력(cli)
2. webpack이 resolve에 있는 다양한 확장자를 인식
3. entry에 있는 모든 파일(resolve의 다양한 확장자를 가진)을 인식
4. module을 사용하여 모든 파일(다양한 확장자)을 js 파일로 변환
5. plugins 를 활용하여 필요한 로직 수행
6. 하나로 합침(bundling)
7. output에서 설정한 경로에 하나로 합친 파일 저장

