---
title: "[Starbucks] CSS Properties"
excerpt: "css properties"
date: 2023-01-23
layout: single
classes: wide
category:
    - starbucks
tag:
    - html
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# CSS 속성(Properties)

요소(Element)란 Openning Tag, Content, Closing Tag로 이뤄진 HTML 요소를 뜻한다. CSS는 이러한 요소에 스타일을 적용한다.

```html
<div>content</div>
```

## 너비 (width, height)

요소의 가로/세로 너비를 지정. auto는 기본적으로 설정된 값으로 자동 설정해주며, 단위는 px, em, vw 등이 있다.

- width, height
  - 요소의 가로/세로 너비를 지정
  - auto: 기본적으로 설정된 값으로 자동 계산
  - 단위: px, em, vw 등
- max-width, max-height
  - 요소가 넓어질 수 있는 최대 가로/세로
  - none: 최대 제한 없음
  - auto: 기본적으로 설정된 값으로 자동 계산 (브라우저가 너비 계산)
  - 단위: px, em, vw 등
- min-width, min-height
  - 요소가 작아질 수 있는 최대 가로/세로
  - 0: 최소 제한 없음
  - auto: 기본적으로 설정된 값으로 자동 계산 (브라우저가 너비 계산)
  - 단위: px, em, vw 등

## 단위

- px: 픽셀
- %: 상대적 백분율
- em: 요소 글꼴 크기
- rem: 루트 요소(html) 글꼴 크기
- vw: 뷰포트 가로 너비 백분율
- vh: 뷰포트 세로 너비 백분율

## 외부 여백 (margin)

- 요소의 외부 여백(공간)을 지정하는 단축 속성 (top, bottom, left, right)
  - 음수 사용 가능
  - 0: 외부 여백 없음
  - auto: 브라우저가 여백 계산 (가로/세로 너비가 있는 요소의 가운데 정렬에 활용)
  - 단위: px, em, vw 등 단위로 지정
  - %: 부모 요소의 가로 너비에 대한 비율로 지정

## 내부 여백 (padding)

- 요소의 내부 여백(공간)을 지정하는 단축 속성
  - 요소의 크기 자체가 커짐
  - 0: 내부 여백 없음
  - 단위: px, em, vw 등 단위로 지정
  - %: 부모 요소의 가로 너비에 대한 비율로 지정

## 테두리 선(border)과 색상

- 요소의 테두리 선을 지정하는 단축 속성
  - border: 선-두께 선-종류 선-색상;
  - 요소의 크기가 커진다
  - border-width
    - 요소 테두리 선의 두께
    - thin, medium, thick
    - 단위: px, em, % 등 단위로 지정
  - border-style
    - none
    - solid
    - dotted
    - dashed
    - double
    - groove
    - ridge
    - inset
    - outset
  - border-color
    - 요소 테두리 선의 색상을 지정하는 단축 속성
    - black, 색상, transparent

- 색상 표현
  - 색상 이름: 브라우저에서 제공하는 색상 이름
  - Hex 색상 코드: 16진수 색상
  - RGB: 빛의 삼원색
  - RGBA: 빛의 삼원색 + 투명도

## 모서리 둥글게(border-radius)

  - 모시를 둥글게 만드는 설정
  - 0: 둥글게 없음
  - 단위: px, em, vw 등 단위로 지정

## 크기 게산 (box-sizing)

- 요소의 크기 계산 기준을 지정
  - content-box: 요소의 내용(content)으로 크기 계산
  - border-box: 요소의 내용 + padding + border로 크기 계산

## 넘침 제어 (overflow)

- 요소의 크기 이상으로 내용이 넘쳤을 때, 보여짐을 제어하는 단축 속성
  - visible: 넘친 내용을 그대로 보여줌
  - hidden: 넘친 내용을 잘라냄
  - scroll: 넘친 내용을 잘라내고 스크롤바 생성
  - auto: 넘친 내용이 있는 경우에만 잘라내고 스크롤바 생성

## 출력 특성 (display)

- 요소의 화면 출력(보여짐) 특성
  - block: 상자(레이아웃) 요소
  - inline: 글자 요소
  - inline-block: 글자 + 상자 요소
  - flex: 플렉스 박스 (1차원 레이아웃)
  - grid: 그리드 (2차원 레이아웃)
  - none: 보여짐 특성 없음, 화면에서 사라짐
  - 기타: table, table-row, table-cell, ... 등

## 투명도 (opacity)

- 요소 투명도 
  - 1: 불투명
  - 0: 완전 투명
  - 0 ~ 1: 0부터 1 사이의 소수점 숫자

## 글꼴

- font-style
- font-weight
- font-size
- line-height (글자는 수직으로 가운데 정렬)
- font-family (마지막에 글꼴 계열을 필수로 작성)

## 문자

- color
- text-decoration
- text-align
- text-indent

## 배경

- background-color
  - transparent
  - 색상
- background-image
  - url("경로")
- background-repeat
  - repeat
  - repeat-x
  - repeat-y
  - no-repeat
- background-size
  - auto
  - px, em, rem 등 단위로 지정
  - cover: 비율을 유지, 요소의 더 넓은 너비에 맞춤
  - contain: 비율을 유지, 요소의 더 짧은 너비에 맞춤
- background-position
  - top, bottom, left, right, center
  - px, em, rem 등 단위로 지정
- background-attachment
  - 요소의 배경 이미지 스크롤 특성
  - scroll: 이미지가 요소를 따라서 같이 스크롤
  - fixed: 이미지가 뷰포트에 고정되어 스크롤 되지 않음

## 배치

- position
  - 요소의 위치 지정 기준
  - top, bottom, left, right, z-index
  - static: 기준 없음
  - relative: 요소 자신을 기준
  - absolute: 위치 상 부모 요소를 기준
  - fixed: 뷰포트(브라우저)를 기준
  - absolute, fixed 지정된 요소는 display 속성이 block으로 지정된 것과 같다

- 요소 쌓임 순서 (Stack order)
  - 어떤 요소가 사용자와 더 가깝게 있는지(위에 쌓이는지) 결정
    1. 요소에 position 속성 값이 있는 경우 위에 쌓임
    2. 1번 조건이 같은 경우, z-index 속성의 숫자 값이 높을 수록 위에 쌓임
    3. 1번과 2번 조건까지 같은 경우, HTML 다음 구조일 수록 위에 쌓임

## 플렉스(정렬) Container

아이템들을 포함하는 컨테이너에 `display:flex;`를 추가해주면 수직으로 정렬된 div 태그들을 수평으로 정렬할 수 있게 해준다.

- 구성 요소와 적용 속성
  - Flex Container
    - display
      - flex: flex container가 수직으로 쌓임
      - inline-flex: flex container가 수평으로 쌓임
    - flex-flow 
    - flex-direction: 나열 방향 (주 축 정렬 방향)
    - flex-wrap: 줄 바꿈
    - justify-content: 정렬 위치
    - align-content: 교차 축의 여러 줄 정렬 방법
    - align-items: 교차 축의 한 줄 정렬 방법
  - Flex Items
    - order: 정렬 우선순위 값 
    - flex 
    - flex-grow: Flex Item의 증가 너비 비율
    - flex-shrink: Flex Item의 증가 너비 비율
    - flex-basis: Flex Item의 공간 배분 전 기본 너비
    - align-self

## 전환

- 요소의 전환(시작과 끝) 효과를 지정하는 단축 속성
  - transition: `transition-property` `transition-duration` `transition-timing-function` `transition-delay`
  - transition-property: 전환 효과를 사용할 속성 이름을 지정
  - transition-duration: 전환 효과의 지속 시간을 지정
  - transition-timing-function: 전환 효과의 타이밍(Easing) 함수를 지정
  - transition-delay: 전환 효과가 몇 초 뒤에 시작할지 대기시간을 지정

## 변환

- 요소의 변환 효과
- transform: `변환함수1` `변환함수2` `변환함수3` `...`;
- transform: `원근법` `이동` `크기` `회전` `기울임`;
