---
title: (0928)module.exports와 exports
date: 2019-09-28 10:10:19
category: TIL
---

`require` 와 `module.exports`, `exports`의 관계를 알아보자

## module

module의 정의를 먼저 설명한 후에야 나머지가 설명 가능하기에
간단하게 정의하자면 기능을 수행하는 최소단위 코드이다
(복잡하게 설명하면 이 주제만으로 시리즈 글을 써야하기에..)

여기서 기능이란? 변수에 값을 저장하고 있는 등 아주 간단한 것도 포함이다  
즉 1줄의 코드로 변수에 값을 저장하고 있어도 모듈이라고 볼 수 있다

### 왜 module이 필요해?

1개의 파일 안에서 코드가 길어지다보면  
가독성이 떨어져 유지 보수가 어려워진다  
이를 해결하기 위해 파일을 여러개로 분리하려는 시도를 했다  
(이때 여러개로 분리한 파일은 보통 module 단위로 분리함)

파일을 여러개로 분리했더니
각 파일에서 dependency 즉, 의존성에서 문제가 발생했다

의존성에서 발생한 문제란  
A 파일에서 필요한 변수를 B 파일이 갖고 있으면,  
A 파일이 다른 파일로 분리된 즉, B 파일에 있는 변수를  
참조할 수 없어서 발생하는 문제이다

이러한 문제를 해결하기 위해 module 즉, 분리한 B 파일을  
A 파일에서 참조할 수 있도록 불러오는 방법이 필요했다

이 불러오는 방법이 바로 `module.exports`와 `require`이다

## require

require 말그대로 필요하다는 뜻으로  
필요한 파일을 지금 여기 파일로 불러오는 방법이다

require는 node의 내장 함수이다  
소스코드는 복잡한 편인데 요약하면 아래와 같다

```js
var require = function(src){
    var fileAsStr = readFile(src)
    var module.exports = {}
    eval(fileAsStr)
    return module.exports
}
```

위 코드를 한줄씩 살펴보자

1. src 는 filePath 이다
2. readFile을 통해 string으로 파일을 저장하고
3. module exports 라는 객체를 만들어서
4. 파일의 내용을 그 객체안에 저장한다  
   (eval 실행의 결과 파일이 실행되고 그 결과 저장이 됨)
5. 파일 내용을 저장한 그 객체를 return 한다

3번을 보면 module exports라는 객체를 만들어서  
라는 말이 등장한다.이게 뭐지?

## module.exports

module.exports는 말그대로 내보내는 방법이다  
뭐를? 분리했던 파일을 즉, module을 내보낸다

node에서 정해준 module을 담아서 내보내는 객체이다  
이러한 객체에 해당 module을 담으면  
다른 file에서 require를 실행한 결과로 그 객체를 return한다  
그 결과, 객체에 담긴 해당 module을 다른 file에서 사용할 수 있다

### require와 module.exports의 관계 정리

module.exports 객체에 분리된 파일(module)을 담아서 내보낸다  
require 함수에 분리된 파일 경로를 넣어 호출하면  
module.exports 객체를 return한다 즉, module을 가져온다

### exports의 정의 & 주의할 점

exports는 module.exports 라는 객체를 참조하는 alias일 뿐이다

exports에 객체를 직접할당하면 즉 `exports = {}`를 하면  
더이상 module.exports를 참조하지 않게 된다  
그 결과 module.exports에는 어떠한 module도 담기지 않게 된다  
그럼 다른 파일에서 module을 require로 가져올 수도 없다
