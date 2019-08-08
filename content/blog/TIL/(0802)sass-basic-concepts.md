---
title: (0802)sass basic concepts
date: 2019-08-02 12:08:41
category: TIL
---

css의 한계를 겪으며 개발자들은 더나은 방법을 고민했다  
그리고 등장한 것이 css 전처리기이다  
그럼 css의 한계부터 전처리기, 나아가 sass까지 살펴보겠다

##css의 한계
project가 커질수록 아래 3가지 한계점은 더욱 크게 느껴진다

- selector 과대 사용
- 연산 기능의 한계
- 구문의 부재

웹에서는 표준 css만 동작하기 때문에 css를 포기할 수는 없다  
이를 감안하면서 위의 한계를 극복할 수 있는 방법이 css 전처리기이다

##css 전처리기(css processor)란?
자신만의 문법을 가지고 css를 생성하도록 하는 프로그램(sass, stylus, less 등)  
프로그램을 사용하면 sass파일을 가져가서 css 파일로 바꾼 후 저장해준다

####css processor 에 고유한 기능

- mixin
- nesting selector
- inheritance selector

####css processor 사용 목적
css 가독성 및 유지보수성 개선

####css processor 사용을 위한 준비
web server에 css compiler 가 설치되어 있어야 함

##sass
sass 는 css processor 중 하나이다

###sass 의 기능

- variables: js의 변수처럼 변수 선언
- nesting: selector 를 nesting
- partials: sass 파일들을 나눠서 모듈화
- import: @import '파일이름' 으로 import

####mixins
마치 함수처럼 css 작성 가능

```js
@mixin transform($property) {
  -webkit-transform: $property;
  -ms-transform: $property;
  transform: $property;
}
.box { @include transform(rotate(30deg)); }
```

위 sass 파일은 아래 css 파일로 변환됨

```js
.box {
  -webkit-transform: rotate(30deg);
  -ms-transform: rotate(30deg);
  transform: rotate(30deg);
}
```

####inheritances

```js
%message-shared {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.message {
  @extend %message-shared;
}

.success {
  @extend %message-shared;
  border-color: green;
}

.error {
  @extend %message-shared;
  border-color: red;
}

.warning {
  @extend %message-shared;
  border-color: yellow;
}
```

위 sass는 아래 css로 변환

```js
.message, .success, .error, .warning {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success {
  border-color: green;
}

.error {
  border-color: red;
}

.warning {
  border-color: yellow;
}
```

####operators

```js
article[role="main"] {
  float: left;
  width: 600px / 960px * 100%;
}

aside[role="complementary"] {
  float: right;
  width: 300px / 960px * 100%;
}
```

위 sass 는 operator가 적용되어 아래 css로 변환됨

```js
article[role="main"] {
  float: left;
  width: 62.5%;
}

aside[role="complementary"] {
  float: right;
  width: 31.25%;
}
```

##references

- sass : https://heropy.blog/2018/01/31/sass/
- css processor : https://developer.mozilla.org/ko/docs/Glossary/CSS_preprocessor
