---
title: "[Design Pattern] MVC Pattern"
excerpt: "mvc pattern (model-view-controller)"
date: 2022-10-13
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

# MVC Pattern

MVC 패턴은 Architecture Pattern으로 분류되는 패턴이며, MVC 패턴은 GoF의 다양한 패턴들로 이루어져있다. 이렇게 두 개 이상의 패턴들로 구성된 패턴을 Compound Pattern이라고 한다. MVC 패턴은 기본적으로 Strategy Pattern, Observer Pattern, Composite Pattern으로 구성되며 다양한 변화가 존재할 수 있다.

이러한 MVC 패턴은 아래와 같은 객체들로 구성된다.

- **Model:** the application object
- **View:** UI (screen representation)
- **Controller:** defines the way the UI reacts to user inputs

## MVC Structure

<img src="/_img/2022-10-13/mvc structure.png">

위 그림에는 각각의 역할이 잘 정리되어 있다. 여기서 한 가지 주목할 점이 있는데 Model과 View의 연결이다. GUI 프로그램에서는 같은 데이터를 서로 다른 화면으로 나타내는 경우가 종종 존재한다. 또한, 데이터보다 화면에 해당하는 부분의 변경이 비교적으로 많이 일어나게 되는데, 이렇게 잦은 변경이 발생하는 부분에 대한 Dependency는 반복적이고 산발적인 수정을 야기한다. 

결론적으로 위 같은 현상을 해결하기 위한 방법으로 MVC 패턴이 고안되었다. MVC 패턴은 핵심 비즈니스 로직을 표현 계층과 조작 계층으로부터 분리하고, 데이터 모델이 표현 계층 속 다양한 화면과 조작 계층 속 다양한 기능 명세들 속에서 공유될 수 있도록 구성된다. 그러나 반대로 변화가 빈번한 표현 계층과 조작 계층으로의 Dependency는 분리되었기 때문에 유연한 확장과 수정을 보장하게 된다.

대표적인 예시로 Non-UI인 Model과 자주 변경되는 View의 Coupling을 끊어주는 방법이 있다. 이를 위해 Observer Pattern을 사용하면 Model은 하나의 Observer가 되어 View에 변경을 알리지만 View는 전혀 알지 못 한 상태로 존재한다.

또한 표현 계층 속 중첩된 Widgets을 구성하기 위해 Composite Pattern이 사용된다. 이로써 원시적인 화면 컴포넌트들을 모으고 동일한 인터페이스로 컴포넌트들을 사용할 수 있도록 구현할 수 있다.

끝으로 View가 사용자의 Gesture를 위임하는 Controller는 Strategy Pattern 구조를 가져 다양한 요청들에 서로 다른 알고리즘을 적용하여 유연한 대응을 보여줄 수 있다. 이처럼 필수적으로 적용되는 패턴 외에도 Factory, Decorator Pattern들을 적용할 수 있다. 핵심은 상황과 목적에 따라 적절한 패턴을 적용하고자 노력하는 것이다.

위에서 설명한 과정을 아래와 같이 간단히 정리해볼 수 있다.

1. User interacts with the View
2. The controller handles the user input
3. The controller updates the model
4. The model notifies the view and it renders the changes
5. The view waits for user interaction

