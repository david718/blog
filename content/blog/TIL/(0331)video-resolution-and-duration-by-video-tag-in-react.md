---
title: (0331)video resolution and duration by video tag in react
date: 2020-03-31 17:03:31
category: TIL
---

```js
let video = document.createElement('video')
let url = URL.createObjectURL(file.data)
video.src = url
video.addEventListener('loadedmetadata', () => {
  const newVideo: UploadVideo = {
    name: file.name,
    size: file.size,
    duration: video.duration,
    resolution: `${video.videoWidth}*${video.videoHeight}`,
    status: 'noStatus',
  }
})
```

1. create video tag to `createElement('video')
2. make url to `createObjectURL` by `blob` data(in this ex name is `file.data`)
3. video src value = url
4. and then `loadedmetadata` event happen
5. so add eventListener callback that gets `video.duration`, `video.videoHeight`, `video.videoWidth`
6. finally, you can get `duration` and `resolution`(width \* height)
