---
title: "[Design Pattern] Bridge Pattern (브릿지 패턴)"
excerpt: "bridge pattern"
date: 2022-10-12
layout: single
classes: wide
category:
    - design pattern
tag:
    - software engineering
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
header:
    teaser: "/_img/thumb-nail/software-engineering.png"
---

# Bridge Pattern

Bridge Pattern의 목적은 추상적 객체 계층과 구현 객체 계층을 나누는 것에 있다. 자칫 개념과 관련된 부분이 구현 과정에서 혼합되는 경우가 있는데, 이를 바로 잡고 분리하기 위해 이 패턴을 사용한다. 이 때, Bridge Pattern을 설명하기 위해 원문에서 사용된 Abstract Object는 Abstract Class와 다른 것임을 유의해야 한다.

- Purpose
  - Defines an abstract object structure independently of the implementation object structure in order to limit coupling
- Use When
  - Abstractions and implementations should not be bound at compile time
  - Abstractions and implementations should be independently extensible
  - Changes in the implementation of an abstraction should have no impact on clients
  - Implementation details should be hidden from the client

## Problem

간단하게 사각형(Rectangle)을 그리는 라이브러리가 **draw_a_line(x1, y1, x2, y2)**, **drawline(x1, x2, y1, y2)** 두 종류가 주어진다고 가정하자. 변화하는 것들을 빼내어 인터페이스로 묶는 방법을 통해 이를 설계하면 아래와 같이 클래스 다이어그램을 작성할 수 있다. 아래와 같은 구조는 drawLine을 구현하는 클래스가 변경되어도 쉽게 대응할 수 있다는 장점을 가진다.

<img src="/_img/2022-10-12/bridge pattern initial example.png">

그런데 이후 새로운 모양인 원(Circle)을 그려야 하는 요구사항이 전달되었다고 가정하자. 이러한 요구사항에 대응하기 위해 Rectangle과 Circle로부터 유도되는 개념인 Shape 상위 클래스를 만들게 된다. 이제 클라이언트가 어떤 라이브러리를 사용하는지는 관계 없이 draw()를 호출할 수 있도록 만들면 아래와 같이 디자인을 설계할 수 있다. 이로써 클라이언트가 Library와 Shape 종류에 종속되지 않게 만들 수 있다.

<img src="/_img/2022-10-12/bridge pattern initial example2.png">

그런데 한 가지 문제가 생긴다. 위와 같은 구조에서 지속적으로 Shape의 종류가 추가되며, 새로운 라이브러리 버전이 나타나면 그 구현의 개수는 Shape 종류와 구현 방법(Library)의 곱으로 늘어나게 된다.

실제로 V1Rectangle이라 하면 V1이라는 구현 라이브러리 버전과 Rectangle이라는 도형의 개념이 Coupling된 것으로 해석할 수 있다. 그렇기 때문에 한 번 더 **개념**과 **구현**을 분리해 Implementation Explosion을 예방할 수 있다.

먼저 **The common things will be represented by abstract, variations will be implemented by concrete**을 적용해보자. 위 예시에서 공통적인 것은 **Shape**과 **Drawing**으로 정리할 수 있다. 이를 공통적인 것으로 두고 변화하는 것들을 수용해보자.

<img src="/_img/2022-10-12/bridge pattern initial example3.png">

이제 Concept에서 Implementation을 알 수 있도록 연관 관계를 설정한다. 이 때, Coupling을 줄이기 위해 Composition을 이용해 구현한다. 반대 방법인 Implementation에서 Concept을 알도록 만드는 것은 Information Expert principle을 위반하기 때문에 권장되지 않는다.

<img src="/_img/2022-10-12/bridge pattern initial example4.png">

이제 클라이언트는 구현 라이브러리나 어떤 종류의 모양인지에 관계 없이 Shape의 draw()를 호출해 그림을 그릴 수 있다. 또한 Shape이나 Drawing 추가에 수반되는 Implementation Explosion으로 이어지지 않도록 관리할 수 있다. 끝으로 이러한 Bridge Pattern은 전략 패턴이나 상태 패턴과 유사한 모습을 지니게 된다.

## Structure of Bridge Pattern

<img src="/_img/2022-10-12/bridge pattern structure.png">