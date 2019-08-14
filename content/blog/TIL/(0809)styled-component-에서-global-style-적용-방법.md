---
title: (0809)styled component 에서 global style 적용 방법
date: 2019-08-09 13:08:64
category: TIL
---

styled-component 공식 웹사이트에 방문하면 helper 에 대한 글이 있다  
https://www.styled-components.com/docs/api#injectglobal

위 글을 참조하여 전체 모든 웹페이지에 style을 적용하는 방법을 찾았다

##createGlobalStyle
위 객체를 사용하면 모든 component가 render될 때 style이 적용된다

```js
import { createGlobalStyle } from 'styled-components'

const GlobalStyle = createGlobalStyle`
  body {
    color: ${props => (props.whiteColor ? 'white' : 'black')};
  }
`

// later in your app

<React.Fragment>
  <GlobalStyle whiteColor />
  <Navigation /> {/* example of other top-level stuff */}
</React.Fragment>
```

여기서 주의할 점은 `<Navigation />` tag이다
이 tag는 react component tree의 top level에 있다  
즉 component가 **1개**여야 한다

만약 `<Navigation />` 자리에 2개 이상의 component가 들어가게 되면  
globalstyle은 적용되지 않는다

따라서 반드시 component tree의 최상위 component는 1개를 사용해야 한다
그 최상위 component 앞에 GlobalStyle 이라는 component를 만들어야 한다
