---
title: (0804)pass custom props in styled-component with typescript
date: 2019-08-09 12:08:96
category: TIL
---

```js
import * as React from 'react'

//<reference path="../../../typings/styled-components/styled-components.d.ts" />

import styled from 'styled-components'

interface Props {
  placeholder: string;
}

const Input =
  styled.input <
  Props >
  `
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`
```

위처럼 코드를 사용하면 기존 html tag에 새로운 attributes  
즉 custom props를 pass 할 수 있게 된다  
자세한 과정은 아래와 같다

1. props를 `interface Props`로 새롭게 지정해준다
2. styled component로 사용하는 html tag뒤에 `<Props>`형태로 할당한다
3. input 에 새로운 props 값을 할당하여 사용한다
