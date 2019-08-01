---
title: (0720)video의 src value 바뀔 때 react에서 re-rendering 하는 방법
date: 2019-07-23 20:07:42
category: TIL
---

react 에서 video tag를 사용하여 local에 저장된  
영상 파일을 불러오려면 아래처럼 코드를 작성해야 한다

```js
<video>
  <source src="file://home/로컬에 있는 영상파일의 경로" />
</video>
```

##react-video
react-video 라이브러리는 html의 video tag를 활용하여 만들어졌다  
react에서 video tag를 사용하여 영상파일을 렌더링해준다

##ComparableVideoViewer에서 영상파일 렌더링하기

아래는 실제 개발중이었던 ComparableVideoViewer 라는 component에서  
영상 파일을 렌더링하는 과정을 나타낸 코드이다

```js{15,16}
interface Props {
  anims: Anim[];
  selectedAnimId: string;
}

const ComparableVideoViewer: React.SFC<Props> = ({ anims, selectedAnimId }) => {
  let path
  anims.forEach(anim => {
    if (anim.id === selectedAnimId) path = anim.path
  })
  console.log(path)
  return (
    <div>
      <div>{path}</div>
      <Player>
        <source src={`file://${path}`} />
      </Player>
    </div>
  )
}

const mapStateToProps = (state: RootState) => {
  return { anims: state.list.anims, selectedAnimId: state.list.selectedAnimId }
}

export default connect(
  mapStateToProps,
  null
)(ComparableVideoViewer)
```

영상 파일을 렌더링하는 과정은

1. VideoList component에서 하나의 video(Anim)를 선택한다
2. selectedAnimId라는 state 가 바뀐다
3. 바뀐 state가 props로 ComparableVideoViewer에게 전달된다
4. state가 바뀌었으므로 ComparableVideoViewer는 리렌더링된다

문제는 여기서 발생했다

> ComparableVideoViewer는 리렌더링 되는데 video tag는 안바뀜

이를 해결하기 위해 stackoverflow에 검색해보니  
video tag에 key 속성의 value 로 src와 똑가튼 file 경로를 할당하라고 했다

위 코드에서 **하이라이팅 된 부분**을 주목하면
Player에 key 속성이 없었음을 확인할 수 있다

##video tag의 key 속성의 value 로 경로(URL)을 할당하자
video안에 source tag의 src value를 수정하더라도  
react는 video가 수정된 것을 인식하지 못한다

> react 입장에서는 video는 수정되지 않았다

따라서 video에 key 속성에 value 를 src와 가튼 file 경로를 할당하여  
src가 바뀌면 key 도 바뀌어서  
react가 video가 바뀐것으로 인식하게 만드는 방법을 사용하면

> video tag가(여기선 Player) src 가 바뀔때마다 리렌더링된다
