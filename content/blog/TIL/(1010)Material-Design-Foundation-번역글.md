---
title: (1010)Material Design Foundation - Environment
date: 2019-10-10 10:10:98
category: TIL
---

Material Design UI 컴포넌트, 부분, 겉보기 등 에 의해  
표현될 수 있는 퀄리티를 정의한다  
또한 거시적, 미시적 관점에서 디자인을 다루는 기초를  
어떤식으로 활용하여 너의 앱을 만들지에 대한 전략이자 방법론이다

이 섹션은 Material 환경과 layout, 상호작용과  
색, 모양, 동작에 관한 하나된 퀄리티 표현 등을 설명한다

이 중 environment 에 대해 설명한다  
이는 아래 3가지 구성요소를 포함한다

- Environment
  - Surfaces
  - Elevation
  - Light and shadows

# Surfaces(표면)

Material Design은 surfaces, depth, shadows 를 활용하여 3차원 특징을 구현한다

## Material environment

material이 놓여진 환경을 고려해서 실제 세계  
즉, 물리 세계와 비슷하게 표현해야 한다

### physical world

물리 세계에서, Objects는 서로 쌓이거나 덧붙여질 수 있다  
그러나 서로를 통과할 수는 없다  
그들은 그림자를 가지며, 빛을 반영한다  
Material Design은 표면의 묘사를 통해 이러한 특징들을 반영한다  
표면과, 그것이 어떻게 3차원 세계에서 움직이는지는 서로 연결되어 있다  
실제 물리 세계에서 그것이 어떻게 움직이는지를 기반으로

### depth

즉 화면을 보는 핸드폰이 놓여져 있는 환경에서  
핸드폰의 두께만큼이 depth 가 된다  
또한 화면 내에서 UI는 빛과 표면, 그림자등으로 깊이를 표현한다

## Properties

Surfaces 는 유지되며 잘 변하지 않는 특징이자 요소이다

### Demensions

material 들은 다양한 x y 좌표를 가지면서  
두께 즉, depth는 1로 통일 되어야 한다

### Shadows

material surfaces는 드리워진 그림자로  
바닥부터의 거리 즉, 높이를 표현한다

### resolution

material은 가까이서 보면 해상도가 올라가서  
보이지 않았던 둥근 모서리 등이 보이기도 한다

### content

content는 material에서 표현된 모양이나 색이다  
content는 material에 두께를 더하지 않는다  
딱 붙어서 표현되어진다

### physical properties

material은 solid하다

- user input 과 interaction은 material을 뚫을 수 없다
- material 은 두께가 있으므로 같은 층에 겹칠 수 없다  
  (겹치려면 다른 층에 겹치도록 그림자로 높이 차이를 표현)
- material 은 서로를 덮은 형태로 존재해서는 안된다
- material 은 gas, gel, liquid 처럼 행동해서는 안된다(시각적으로)

### transforming material

- material은 모양을 바꿀 수 있다
- opacity(투명도)를 조절할 수 있다
- 면을 따라서 크기가 바뀔 수 있다
- 구부리거나 접지 마라
- 2개 이상의 surfaces가 1개로 합쳐질 수 있다

### movement

- 어디서든지 나타났다가 사라질 수 있다
- 같은 depth에서는 자유롭게(x, y) 움직일 수 있다
- 서로 움직임이 연관될 수 있다
- interaction 할 때만, z-축(depth)은 변할 수 있다

## Attributes(properties 의 구체적인 값)

기본적인 material surfaces

- opaque white
- 1dp thickness
- cast shadow

### behavior

- rigid surfaces : 모든 상호작용에서 같은 size
- stretchable surfaces : 확장 혹은 수축한다
- pannable surfaces : scroll 가능한 rigid surfaces

각각 다른 interaction이 있는 surfaces 를 composite 할 수 있다

### stretchable surfaces

limit 되기 전에 늘어날 수 있다  
전체 surfaces 가 rigid 가 될 때  
surfaces는 수직, 수평, 대각선 모두 늘어날 수 있다  
interaction은 수직, 수평 한방향으로만 stretch하게 한다

### surface positioning and movement(x/y)

surfaces 는 fixed 될 수 있다  
또한 어느 방향(x/y)으로도 움직일 수 있다  
또한 surfaces 는 서로 movement에  
영향을 줄 수도 있고 독립적일 수도 있다

### surface opacity

- transparent
- semi-transparent
- opaque

투명한 surface 위에 content 가 읽기 쉽도록  
surface background에 treatment 가 필요하다

투명하기 때문에 다른 surface 의 content가 이동하여  
겹치게 되면 기존 surface 의 content가 읽기 어려워진다

### scrim

scrim으로 surfaces 에 content 를  
덜 중요하게 보이도록 만들 수 있다

- darkening or lightening surface and content
- reducing opacity surface and content

# Elevation

Elevation 은 2개의 surface 사이에 상대적인 z 축 거리다  
즉, depth 의 상대적인 차이값이다

## Elevation in Material Design

각 Material 마다 다른 elevation 값을 가진다  
그 값을 정하는 기준이 있다

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

각 compoent 별로 default elevation value 값이 있다
표를 참고하자

# Light and shadow

빛을 가리는 Material surface 는 shadow를 만든다

## Light

### Light and shadows

Material Design environment 에서 가상의 빛이 UI 를 비춘다  
shadow 는 elevation 을 표현한다  
elevation 즉, z 축의 길이값이 클수록 shadow 도 커진다
