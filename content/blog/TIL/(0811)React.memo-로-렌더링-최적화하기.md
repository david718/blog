---
title: (0811)React.memo 로 렌더링 최적화하기
date: 2019-08-11 09:08:85
category: TIL
---

렌더링 최적화에 대해 알아볼 것이다  
글 내용의 순서는 아래와 같다

1. 성능 최적화에 대한 설명(react에서)
2. component의 리 렌더링 되는 경우
3. component 쪼개기(쪼개는 기준)
4. React.memo 사용 방법
5. React.memo 실제 코드에 적용

##1. 성능 최적화(functional component 호출 제한)
Optimizing performance 즉, 성능 최적화는 어떤 의미인가?  
성능 최적화는 비용 절약이다  
그럼 react에서 성능 최적화는 어떤 의미인가?  
렌더링 최적화가 react 성능 최적화 중 1가지가 될 수 있다

react의 functional component를 렌더링할 때 함수를 호출한다  
이것은 `비용`이 크다  
하지만 처음 1번은 무조건 호출해야 한다(렌더링이 되야하므로)  
처음을 제외하고 다시 렌더링 되는 경우를 파악하여,  
꼭 필요할때만 다시 렌더링 되게 조건을 제한한다면  
함수를 호출할 때 드는 큰 `비용`을 절약할 수 있다

##2. functional component가 재호출(리 렌더링)되는 경우
그럼 component는 언제 다시 렌더링 되는가?

1. props가 바뀔 때
2. setState 실행 시

위 2가지 경우 component는 다시 렌더링 된다
1번 props가 바뀔 때에서 이전 props와 다음 props 중  
달라진 값을 비교하여 달라졌을 때만 해당 component를 호출한다면  
비용을 절약할 수 있다. 즉, 성능 최적화가 가능해진다

##3. component 쪼개기
component가 많은 html tag를 렌더링 할수록  
props가 바뀔때 다시 렌더링 될 필요가 없는 html tag까지 렌더링된다

그러므로 component가 최소한의 html tag만을 가지도록 쪼개자  
여기서 최소한이란 component가 의미를 가지는  
즉, 변하는 data를 가지고 렌더링하는 가장 작은 html tag 개수이다

```js
<SComparableVideoViewer>
  <SButton width={60} onClick={startAnim}>
    Start
  </SButton>
  <SButton width={60} onClick={pauseAnim}>
    Pause
  </SButton>
  <SAnimTitle>{animTitle}</SAnimTitle>
  <canvas ref={canvasRef} />
  <video
    style={{ display: 'none' }}
    ref={videoRef}
    src={`file://${selectedPath}`}
    controls={true}
  />
</SComparableVideoViewer>
```

기존 component는 위에서 보듯이 4개의 `html tag`를 렌더링하고 있었다

- button
- title(div)
- canvas
- video

이를 아래와 같이 2개의 `html tag`를 렌더링하도록
`VideoControlBar`라는 component를 새로 만들어 component를 쪼갰다

```js
<SComparableVideoViewer>
  <VideoControlBar
    startAnim={startAnim}
    pauseAnim={pauseAnim}
    animTitle={animTitle}
  />
  <canvas ref={canvasRef} />
  <video
    style={{ display: 'none' }}
    ref={videoRef}
    src={`file://${selectedPath}`}
    다
    controls={true}
  />
</SComparableVideoViewer>
```

(`VideoControlBar`에는 `button`과 `title(div)`이 있다)

```js
<>
  <SButton width={60} onClick={startAnim}>
    Start
  </SButton>
  <SButton width={60} onClick={pauseAnim}>
    Pause
  </SButton>
  <SAnimTitle>{animTitle}</SAnimTitle>
</>
```

이 후 `VideoControlBar` component를 렌더링 최적화했다
즉, 기존 `ComparableVideoViewer` component에서  
리 렌더링 되어야 했던 tag는 `canvas`와 `video` 2개의 tag였다

따라서 이를 제외한 tag를 component로 묶어 쪼갠 후
해당 component(`VideoControlBar`)에 React.memo를 사용하여  
props가 바뀌지 않을 때는 다시 렌더링 되지 않도록 수정했다

##React.memo 동작 원리
React.memo는 React component와 callback  
2개 parameter를 받는다

```js
const FunctionalComponent = React.memo(({...props}) => {
  return (
    //html tag
  )
}, (prevProps, nextProps) => {
  if('리 렌더링 해야하는 조건') {
    return false;
  }
  return true;
})
```

위 코드에서 확인할 수 있듯이 2번째 parameter인 callback은  
prevProps와 nextProps 즉 이전 props와 다음 props를 parameter로 받는다

그리고 `true` 혹은 `false`를 return하는데  
이때 `true`를 return하면 component가 리렌더링 **되지 않는다**
`false`를 return하면 component가 리렌더링 **된다**  
(shouldComponentUpdate와 **반대로** 동작)

따라서 이 callback에서 이전 props와 다음 props를 비교하여  
리렌더링 해야하는 조건을 지정해준다

##5.React.memo 실제 코드에 적용(`VideoControlBar` component)

```js
const VideoControlBar: React.SFC<Props> = React.memo(
  ({ startAnim, pauseAnim, animTitle }) => {
    return (
      <>
        <SButton width={60} onClick={startAnim}>
          Start
        </SButton>
        <SButton width={60} onClick={pauseAnim}>
          Pause
        </SButton>
        <SAnimTitle>{animTitle}</SAnimTitle>
      </>
    )
  },
  (prev, next) => {
    if (prev.animTitle !== next.animTitle) {
      return false
    }
    return true
  }
)
```

`VideoControlBar` component의 props는 3개다

- startAnim
- pauseAnim
- animTitle

이 중 animTitle이 바뀌면 리렌더링을 해야한다
(바뀐 title을 렌더링해서 표시해주어야 하기 때문에)

따라서 callback이 false를 return하는  
즉, 리렌더링 해야하는 조건에  
이전 animTitle과 다음 animTitle이 다를 때를 설정한다

그 결과 animTitle의 값이 바뀌면  
`VideoControlBar`는 리렌더링 된다
(그렇지 않은 경우는 callback이 true를 return하므로  
리렌더링 되지 않는다)
