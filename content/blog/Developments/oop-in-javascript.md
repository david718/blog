---
title: OOP in Javascript
date: 2019-05-16 15:05:58
category: Developments
---

**함께보면 이해가 쏙쏙**  
https://youtu.be/Ws_GHE5D62s

## 객체지향 프로그래밍

### 정의

- 실제 세계에 기반한 모델을 만들기 위해 추상화를 사용하는 프로그래밍 패러다임
- 관계성 있는 객체들의 집합이라는 관점으로 접근하는 소프트웨어 디자인
- 객체는 data를 처리하거나 message를 주고받을 수 있다(작은 기계 역할)
- 유연하고 유지보수성 높은 프로그래밍을 가능하게 해주는 방식

### 용어

- 개체
  - Class - 객체의 특성을 정의
  - Object - Class의 인스턴스
  - Property - 객체의 특성
  - Method - 객체의 능력
  - Constructor - 인스턴스화 되는 시점에서 호출되는 method
- 특성
  - Abstraction - 특정 세부사항은 숨기고(generalization) 대상의 필수 기능(specialization)표시하는 것(보통 interface를 사용함)
  - Inheritance - Class는 다른 Class들로부터 Property를 상속받을 수 있다
  - Encapsulation - Class가 가진 property와 method를 묶는다(capsule화). 왜? 외부에서 해당 property에 접근이 불가능하도록(은닉화 → closure 활용)
  - Polymorphism - 다른 Class 들이 같은 method나 property로 정의될 수 있다

## Prototype-based programming(Class 대신)

- Prototype 을 활용하여 동작 재사용(behavior reuse == Inheritance)이 수행된다

## OOP in JS

- JS는 core에 몇개의 객체가 있음
  - Math, Object, Array, String 등
- 모든 객체는 Object 객체의 instance 이므로 Object의 method, property를 inheritance 받는다

### 용어(자바스크립트에서 무슨 뜻인지)

- 개체

  - Class - function 으로 정의 가능(Constructor가 Class라고 볼 수 있음)
  - Object - Class의 인스턴스(Constructor function의 실행 결과)
  - Property - 객체의 특성(Constructor 내부의 변수 this.property = value 로 정의)
  - Method - 객체의 능력(Constructor의 prototype.method = function 로 정의)

    this 주의해서 사용!(this는 method 호출 시점에서 value가 결정되므로)

  - Constructor - 인스턴스화 시점에서 호출되는 method(class function 자체가 constructor)

- 특성

  - Inheritance

    [](https://www.notion.so/5fa7f7c314b74b529205cb3ffa532c56#c60c5d6ebc5c46359ed1312f09847e14)

    prototype을 활용하여 진행(prototype을 이용해야 모든 instance에게 전달)

```js
            function Child ( ... ) {
            	Parent.apply(this, arguments); //property 상속 과정
            }

            Child.prototype = Object.create(Parent.prototype); //method 상속 과정
            Child.prototype.constructor = Child //재할당 안해주면 Parent가 constructor
```

    - Encapsulation

        [](https://www.notion.so/5fa7f7c314b74b529205cb3ffa532c56#e373e86d1c5f45709483237995097070)

        객체의 data가 직접 노출되지 않아야 함(임의로 잘못된 값 수정 불가)

        - 문제 상황(숫자를 넣으면 이름이 갑자기 숫자가 됨)

```js
var person = {
  fullName: 'david hwang',
}

alert(person.fullName) // david hwang
person.fullName = 31
alert(person.fullName) // 31
```

        - 해결책 - 한마디로 - property를 숨긴다(은닉화 → closure)

```js
var person = {
  fullName: 'david hwang',
  setFullName: function(newValue) {
    if (typeof newValue !== 'string') {
      alert('Invalid Name')
    } else {
      this.fullName = newValue
    }
  },
  getFullName: function() {
    return this.fullName
  },
}

alert(person.getFullName()) // david hwang
person.setFullName('Rockheung')
alert(person.getFullName()) // Rockheung
person.setFullName(31) // Invalid Name
alert(person.getFullName()) // Rockheung
```

    - Abstraction

        [](https://www.notion.so/5fa7f7c314b74b529205cb3ffa532c56#65d0d9ca5f74470a9f8dd170a272d358)

        - 일반적인 상황 : 특정 세부사항은 숨기고(generalization) 대상의 필수 기능(specialization)표시하는 것

```js
const add = (a, b) => a + b

const a = add(1, 1) //2
const b = add(a, 1) //3
const c = add(b, 1) //4

const editAdd = a => b => a + b
const inc = editAdd(1)
inc(1) // 2
inc(2) // 3
```

        - in javascript 한마디로 class의 property와 method의 구조를 재사용(상속)가능하게

```js
class ClassName {
  constructor(init1, init2) {
    this.prop1 = init1
    this.prop2 = init2
  }
  get props() {
    return [this.prop1, this.prop2]
  }
  set props(val) {
    ;[this.prop1, this.prop2] = val
  }
  methodInst() {
    // Instance level method. //필수기능은 상속가능하도록(specialization)
    // Do something...
  }
  static methodStat() {
    // Class level method.  //세부사항은 숨긴다 generalization
    // Do something...
  }
}

const inst = new ClassName()
inst.props = [1, 2]
inst.methodInst()
ClassName.methodStat()
```

    - Polymorphism

        [](https://www.notion.so/5fa7f7c314b74b529205cb3ffa532c56#1a635089b289419c93c58dcd696f9ff7)

        method가 역할마다 이름을 다르게 갖지 않고 공통 역할을 묶어서 쓸 수 있다. 그 결과 재사용성이 높아진다. (다양한 자료가 타입을 가질 수 있다)

        - 일반

```js
//숫자를 문자열로 바꾸는 경우
string = StringFromNumber(number)

//날짜를 문자열로 바꾸는 경우
string = StringFromDate(date)

//숫자를 문자열로 바꾸는 경우
string = number.StringValue()

//날짜를 문자열로 바꾸는 경우
string = date.StringValue()
```

        - in javascript 한마디로 method 공유!

```js
function Person(age, weight) {
  this.age = age
  this.weight = weight
}

Person.prototype.getInfo = function() {
  return (
    'I am ' + this.age + ' years old ' + 'and weighs ' + this.weight + ' kilo.'
  )
}

function Employee(age, weight, salary) {
  this.age = age
  this.weight = weight
  this.salary = salary
}
Employee.prototype = new Person() //여기서도 getInfo할수있지만 salary빠짐

Employee.prototype.getInfo = function() {
  return (
    'I am ' +
    this.age +
    ' years old ' +
    'and weighs ' +
    this.weight +
    ' kilo ' +
    'and earns ' +
    this.salary +
    ' dollar.'
  )
}
```
