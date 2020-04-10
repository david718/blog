---
title: (0410)add props to react component instance
date: 2020-04-10 13:04:62
category: TIL
---

이미 호출된 component 는 instance 이다  
이 react component instance 에 props 를 add 하려면  
호출할 때 `<component props={props} />`  
형태로 호출하면 props add 가능하다

하지만 아래 코드에서처럼  
이미 호출된 react component(content)에 add props 하려면

> `React.cloneElement` 를 호출해야 한다

https://ko.reactjs.org/docs/react-api.html#cloneelement

```js{13}
const ModalContainer = ({ title, content }: ModalContainerProps) => {
  const [visible, setVisible] = useState(false)

  const showModal = (e: any) => {
    e.stopPropagation()
    setVisible(true)
  }
  const closeModal = (e: any) => {
    e.stopPropagation()
    setVisible(false)
  }

  //closeModal 을 content component instance 에 props로 add 하기
  const newContent = React.cloneElement(
    content,
    { ...content.props, closeModal },
    content.children
  )

  return (
    <>
      <Button type="primary" onClick={showModal}>
        {title}
      </Button>
      <Modal title={title} visible={visible} onCancel={closeModal}>
        {newContent}
      </Modal>
    </>
  )
}
```

위 코드를 보면 `ModalContainer` 가 modal 안에 들어갈  
content 를 props 로 받는 구조이다  
이때 content 는 이미 호출된 react component instance 이다

이러한 content에 closeModal 이라는 event handler를  
props로 add하고 싶을때 `React.cloneElement`를 호출한다

## what arguments needed for executing `React.cloneElement()`

```js
//closeModal 을 content component instance 에 props로 add 하기
const newContent = React.cloneElement(
  content,
  { ...content.props, closeModal },
  content.children
)
```

`React.cloneElement`는 3가지 arguments 가 필요하다

1. add props 를 할 component instance
2. add 할 모든 props
3. component instance 의 children

위 예시에서는 아래와 같이 들어갔다

1. content
2. { ...content.props, closeModal }
3. content.children

여기서 주의할 점은  
2번 add 할 props의 경우  
object 형태로 위에 2번처럼 arguments로 주어야 한다

호출된 newContent에서
`newContent.closeModal`을 할 경우  
props로 잘 전달받았음을 확인할 수 있다.
