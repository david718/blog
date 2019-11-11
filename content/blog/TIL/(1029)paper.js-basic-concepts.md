---
title: (1029)paper.js basic concepts
date: 2019-10-29 12:11:44
category: TIL
---

paper.js 는 vector graphics scripting framework  
HTML5 canvas elements 를 기반으로 동작한다

### vector?

한 position 에서 다른 position 까지 거리를  
x, y 좌표로 표현하거나 각도, 길이로 표현하는 방법

## hierarchy and methods

paperjs 는 아래와 같은 hierarchy 를 가진다

- Project
  - Layer
    - Group
      - Raster
      - Path
      - Symbol

### Raster

만약 Raster(paperjs의 method)없이 drawImage 하려면  
아래와 같은 과정을 거쳐야 한다

```js
const image = new Image()
const canvas = document.createElement('canvas')
const ctx = canvas.getContext('2d')
const src = 'https://~~~image.jpg'

image.onload = function() {
  canvas.width = image.width
  canvas.height = image.height
  ctx.drawImage(image, 0, 0)
}

image.src = src
```

하지만 raster 를 사용하면 아래 한줄로 끝난다

```js
const raster = new Raster('imageTagIdName')
```

### Symbol

Symbol method 는 Class object 라고 생각하면 된다  
Symbol 의 instance 는 Symbol의 definition을 바꾸면 한번에 바뀐다

```js
const circle = new Path.Circle(new Point(100, 100), 60)
const circleSymbol = new Symbol(circle) // create symbol

circleSymbol.place(view.size.multiply(Point.random())) // place instance
circleSymbol.definition.rotate(1) // change definition
```

### Event handler functions

- onMouseDown / onMouseUp
- onMouseDrag
- onFrame(60 frames per second)

### Event Object

- event.count : counts frame number
- event.point : point at which event is fired
- event.delta : vector, measuring change in position

```js
circle.onMouseDrag = e => {
  console.log(e.delta) // vector value
  circle.position += e.delta
}
```
