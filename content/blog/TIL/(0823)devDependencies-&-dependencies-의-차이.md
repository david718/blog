---
title: (0823)devDependencies & dependencies 의 차이
date: 2019-08-23 14:08:87
category: TIL
---

package.json을 보면 dependencies와 devDependencies가 나뉘어있다  
둘의 차이를 알아보자

##dependencies
내가 packaging한 module을 다른 사람들이 다운받아서 활용할 때  
설치가 필요한 library 모음

yarn install 하면 local의 node_modules 폴더 안에 설치될 library 목록이다

##node_modules
node package 들이 설치될 directory로  
기본적인 node의 module들이 설치되어 있다

##devDependencies
package module을 다운받아서 활용할 사람들은  
test, transpile 혹은 문서 작성을 위한 library는 다운받을 필요가 없다

이러한 dependecies 즉, 개발할 때만 필요한 dependencies는  
devDependencies에 추가하여 사용한다
