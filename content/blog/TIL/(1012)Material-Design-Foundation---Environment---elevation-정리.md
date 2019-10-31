---
title: (1012)Material Design Foundation - Environment - elevation 정리
date: 2019-10-12 12:10:66
category: TIL
---

Elevation 은 2개의 surface 사이에 상대적인 z 축 거리다  
즉, depth 의 상대적인 차이값이다

elevation value examples

- Nav drawer : 16dp
- App bar : 4dp
- Card : 1dp to 8dp
- FAB : 6dp
- Button : 2dp to 8dp
- Dialog : 24dp

## Elevation in Material Design

### Measuring elevation

elevation 은 Material surfaces 들 사이 거리로 측정한다
또한 z 축의 길이이므로 shadows 로 표현한다

### elevation system

모든 surfaces, components 들은 elevation value를 가진다

elevation 값이 다른 surfaces 를 표현하는 방법은 3가지다

- shadow
- 다른 색으로 surfaces 채우기
- opacity

### resting elevation

resting elevation은 elevation value 의 시작 값이다  
component는 resting elevation 에서 시작해서 움직인다
각 component 마다 특정한 resting elevation 값이 있다

예를 들어 버튼의 resting elevation은 2dp 이지만  
클릭하면 8dp 로 값이 바뀐다

### elevation interference

component 들이 움직여서 elevation 이 겹치면 안된다  
이를 위해 특정 길을 따라 component 들이 움직이도록 설정한다

## Depicting elevation

elevation 값은 아래 3가지 조건을 고려해서 정하자

1. surface edges 는 surface 주변과 대조되도록
2. surface 들이 겹쳐있다면 움직여서 겹치지 않도록
3. 다른 surface 들과의 거리가 보이도록

### edge

- color
- opacity

위 2가지를 다르게 주어서 edge 를 대조되게 할 수 있다

### overlap

겹침을 보여줄 때도 elevation 의 차이를 분명하게 보여주자

### Distance

component 가 다른 component 위에 올라와 있다면  
아래 깔린 component의 배경을 scrimmed 하게 처리하자

shadow를 활용하여 elevation 을 표현할 수 있다
(design 용도로만 shadow를 사용하지는 말자)

### motion and elevation

motion 에 의해 elevation이 바뀌는 것을 표현하는 방법들

- changes in shadows
- displaying overlap
- pushing
- scaling
- parallax

## Elevation hierachy

다른 elevation 값을 가지는 content 들

앞에 있는 surface 일 수록

1. 더 중요한 content
2. Focus attention(dialog)
3. 뒤에 surface를 control 한다(app bar)

### content on coplanar surfaces

동일 평면상에 있으면 같은 중요도를 가진다

## Default elevations


