---
title: (0922)Nextjs에서 _app.js로 Layout 만들기
date: 2019-10-14 11:10:25
category: TIL
---

nexjs를 활용해서 website를 만들 때 유용한 팁이 있다  
pages 폴더 안에서 `_app.js` file을 활용하는 것이다

## `_app.js`는?

Component 라는 props를 받는 특수한 pages 안에 file이다  
nextjs에서 이미 만들어 놓은 api 이므로  
Component 를 활용하면 된다  
이때 Component는 pages 폴더 안에 모든 file들을 말한다  
즉, 모든 page들을 말한다

## Layout 만들기

layout은 모든 page들에서 공통으로 활용되는 부분이다  
따라서 모든 page를 불러와서 적용할 수 있도록 만들어야 한다  
모든 page는 `_app.js`의 Component props를 활용하여  
접근할 수 있으므로 `_app.js`에서 layout을 작성한 후에  
모든 page에 적용하면 가능하다

```js
const app = ({ Component }) => {
  return (
    <>
      <Head>
        <title>NodeBird</title>
        <link
          rel="stylesheet"
          href="https://cdnjs.cloudflare.com/ajax/libs/antd/3.23.3/antd.css"
        />
        <script src="https://cdnjs.cloudflare.com/ajax/libs/antd/3.23.3/antd.js" />
      </Head>
      <AppLayout>
        <Component />
      </AppLayout>
    </>
  )
}

export default app
```

위 코드를 보자 `_app.js`의 component 함수이다  
Head와 AppLayout 2개가 전체 app의 Layout이므로  
모든 page에 적용해야 한다

그 방법은 위에서처럼 props인 Component를 가져온다  
그 후 Layout이 적용되야하는 자리 안에서  
JSX처럼 `<Component />`로 호출하여 tag를 return한다
