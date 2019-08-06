---
title: (0726)canvas로 영상 비교하는 react component 만들기
date: 2019-07-26 14:08:53
category: TIL
---

##Comparable Video Viewer component 만드는 순서
canvas와 video tag를 활용하여 영상을 비교하며 볼수 있는  
react component를 만드는 과정에 대해 설명하는 글이다

1. **canvas로 도형 렌더링하기:** 두 영상을 비교해서 보여줄 때 각 영상의 넓이를 조절하는 bar element를 그리기 위함이다
2. **video tag와 setInterval을 활용하여 canvas위에 영상 렌더링하기:** 바뀌는 video tag의 img src 값을 가져와서 canvas에 capture를 반복적으로 다시 렌더링하는 방법을 사용한다
3. **video를 2개의 image로 나눠서 canvas 위에 각각 렌더링하기:** drawImage를 활용한다
4. **bar element를 움직일 수 있도록 만들기:** useEffect를 활용하여 bar element의 x좌표를 state로 관리한다
5. **bar element의 위치에 따라서 2개의 image 보여주는 비율 조정하기:** drawImage를 module화 하여 해결한다

##1. canvas 로 도형 렌더링하기
canvas 는 HTML5에서 등장한 tag이다  
canvas 위에는 2d 혹은 3d 의 다양한 도형과 이미지파일을 렌더링할 수 있다

사각형을 렌더링 과정은 아래와 같다

1. canvas의 넓이와 높이를 지정한다
2. `getContext()` 를 실행하여 2d 혹은 3d 를 결정하는 context 값을 얻는다
3. context.beginPath()로 그리는 연필을 시작점에 위치시킨다
4. fillStyle 에 그릴 도형의 색을 할당한다
5. context.rect(...)로 그릴 사각형의 크기를 결정한다
6. context.fill 로 사각형을 그린다

위 과정을 통해 알 수 있듯이  
canvas는 context를 가진다  
이 context 위에서 다양한 도형 혹은 이미지파일을 그리게 된다

이 canvas 위에서 움직이는 도형을 그리는 방법은  
setInterval을 사용하여 변화한 값을 반영하여  
반복적으로 다시 렌더링하는 것이다

##2. video 와 setInterval로 canvas에 영상 렌더링하기
react hook과 함께 setInterval을 사용하기 위해서는 `useEffect`가 필요하다
useEffect는 componentDidMount와 componentDidUpdate, componentWillUnmount까지 합친것과 비슷하다

```js
useEffect(() => {
  //componentDidMount
  return (
    //componentWillUnmount
  )
}, ['이 배열에 추가한 item이 바뀔때, componentDidUpdate 실행'])
```

위 코드에서처럼 componentDidMount 에서 setInterval을 설정하고  
component가 unmount 될 때 clearInterval 해준다  
또한 바뀌는 video와 state 를 배열에 추가한다  
이를 통해 마치 setInterval을 실행하는 것처럼 동작하게 만든다

실제 코드를 보자

```js{2,7,8}
useEffect(() => {
  const interval = setInterval(() => {
    if (video) {
      devineVideoToCanvasWithBar(video, canvasRef.current, video.videoWidth / 4)
    }
  })
  return () => clearInterval(interval)
}, [video, barX])
```

**첫번째 하이라이팅된 부분**  
setInterval이 실행되었다 이때 canvas에 video 캡쳐이미지를 그리는 함수  
`devineVideoToCanvasWithBar`가 실행되었다

**두번째 하이라이팅된 부분**  
component가 unmount될 때 실행되는 부분으로  
clearInterval 을 실행하고 있다

**세번째 하이라이팅된 부분**  
배열안에 video와 barX 2개의 item 이 있다  
video는 video tag이다  
barX는 bar element의 x 좌표이다
이 두가지가 변할때 useEffect는 실행된다

동작 순서는 아래와 같다

1. setInterval로 video 현재 송출 이미지를 캡쳐하여 canvas에 그린다
2. component가 unmount 된다
3. clearInterval로 정리한다
4. component가 mount 되면 setInterval을 다시 실행한다(이후 반복)

##3. video를 2개 이미지로 나눠 캡쳐한 후 canvas에 렌더링하기

![](images/Canvas_drawimage.jpg)

그림에서 확인할 수 있듯이 Source image가 video tag 화면이다  
Destinaion canvas에 그리려고 한다

이때 주의할 점은 캡쳐해오는 image와 그릴 image의 넓이를 변수로 두는 것이다  
왜냐하면 움직이는 bar의 x좌표 즉 barX에 따라 image의 넓이가 바뀌기 때문이다

이 drawImage 를 활용하여 아래의 과정을 거쳐 video를 2개로 나눠 렌더링했다

![](images/devineVideoTocanvas.jpg)

##4.
