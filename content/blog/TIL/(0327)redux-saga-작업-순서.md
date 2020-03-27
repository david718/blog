---
title: (0327)redux saga(async action) 작업 순서
date: 2020-03-27 21:03:53
category: TIL
---

1. action type을 선언한다(REQUEST, SUCCESS, FAILURE)
2. action creator function 을 선언한다(request, success, failure)
3. action의 payload 를 parameter 로 받아 api request 를 call 하는 async API function 을 선언한다
4. API function 의 결과에 따라 success 혹은 failure 을 put 하는 saga function 을 선언한다
5. action type 을 `takeEvery`하여 saga function을 호출하는 watch function 을 선언한다
6. watch function을 fork 하여 지켜보는 rootSaga function 을 선언한다
7. reducer 에서 action 에 따라 store 값을 변화시킨다
8. component 에 가서 `useDispatch`를 통해 action 을 dispatch 한다
   (필요하다면 `useSelector`를 통해 store 값을 불러온다)
