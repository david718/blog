---
title: (0804)pass custom props in styled-component with typescript
date: 2019-08-09 12:08:96
category: TIL
---

button 을 styled component로 styling해서 사용할 때  
button 에 width라는 attribute 를 주려고 하는데  
typescript를 사용한다면 에러가 발생한다

```js
const SButton = styled.button`
  font-size: 1.5em;
  text-align: center;
  color: gainsboro;
  width: ${props => props.width}px;
`

<SButton width={80}>click me</SButton>
```

위와 같이 SButton 이라는 styled-component에  
width 값을 할당하려고하면 tslint가 에러를 잡는다  
button tag에는 원래 width라는 attribute가 없기 때문이다  
따라서 아래처럼 interface로 props를 선언하여 할당해줘야 한다

```js
interface Props {
  placeholder: string;
}

const SButton = styled.button<Props>`
  font-size: 1.5em;
  text-align: center;
  color: gainsboro;
`

<SButton width={80}>click me</SButton>
```

위처럼 코드를 사용하면 기존 button tag에  
새로운 attribute로 width를 할당할 수 있게 된다  
즉 custom props를 pass 할 수 있게 된다  
자세한 과정은 아래와 같다

1. props를 `interface Props`로 새롭게 지정해준다
2. styled component로 사용하는 html tag뒤에 `<Props>`형태로 할당한다
3. button 에 새로운 props 값을 할당하여 사용한다
