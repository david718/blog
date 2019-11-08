---
title: (1012)Material Design Foundation - Layout
date: 2019-10-12 12:10:66
category: TIL
---

## Understanding layout

platform 을 모두 고려하는 layout을 생각한다  
uniform elements 와 spacing 을 활용하자

### principles

- predictable : UI는 각 element마다의 고유 영역을 통해 예측가능해야 한다
- consistent : gird, keylines padding 을 일정하게 유지해야 한다
- responsive : user, device, screen 에 맞춰 반응형이어야 한다

### structure

layout 각 요소들의 z축 값은 4dp 단위로 구분되어  
깊이를 표현하고 있다  
작은 요소들은 4dp grid 에 위치하는 것이 일반적이다

## pixel density

screen 의 화질, 즉 pixel 밀집도는 platform 에 달려있다

### screen density variations

고화질 스크린은 pixel 밀집도가 높다  
그 결과 같은 pixel 수를 가진 UI도  
고화질 스크린에서는 더 작게, 저화질 스크린에서는 더 크게 나타난다

### density independence

density 가 다른 screen 에서도 같은 크기로 나타나는  
UI 는 density independence 를 가진다

Material UI 는 density independence pixel 을 단위로  
스크린에 따라서 UI가 차지하는 면적넓이가 달라지지 않는다

## Responsive layout grid

screen 사이즈와 상관없이 항상 같은 layout 을 보장한다

### columns, gutters, margins

- Columns
  - content 는 column 을 포함한다
  - column width 는 % 로 정한다
  - mobile 은 4개, web 은 8개로 정하자
- Gutters
  - column 사이 공간으로 content를 구분한다
  - column 보다 넓이값이 크면 좋지 않다
  - mobile 은 16dp, web 은 24dp
- Margins
  - screen 양 옆 빈공간
  - content 에 비해 너무 크지 않도록 하자
  - mobile 은 16dp, web 은 24dp

## UI regions

break point 마다 화면에 맞춰 UI 가 차지하는 영역이 다르다  
mobile UI 의 일관성을 잃지 않으면서 web UI 영역을 설정하자

### permanent UI regions

navigation bar 같이 responsive 하지 않은 고정된 UI

### persistent UI regions

고정값을 가지고 있는 UI region 나타나거나 숨기도 함

### Temporary UI regions

UI region 이 나타나도 다른 UI 넓이에 영향을 주지 않는 영역

## Spacing methods

baseline gird 와 keyline, padding, incremental spacing, container, touch targets 등이 공간을 가지는 규칙이다

### Baselines

8dp grid 모든 components는 8dp 사각형의 기본 grid를 가진다

ICON, typography 등은 4dp를 가지기도 한다

### spacing

- keylines : content 시작점이 수직으로 정렬된 라인으로, 화면 왼쪽 끝에서부터 떨어진 거리에 있다
- padding : UI element 사이 공간을 표현한다
- vertical spacing : 각 element 들이 표현되는 bar 의 높이값
  - status bar : 24dp
  - app bar : 56dp
  - list item : 88dp
- grid : column 을 넓게 했으면 gutter 를 좁게, 반대로도 가능, 하지만 둘다 좁게는 피하자
