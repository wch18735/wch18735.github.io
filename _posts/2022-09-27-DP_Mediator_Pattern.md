---
title: "[Design Pattern] Mediator Pattern (중재자 패턴)"
excerpt: "mediator pattern"
date: 2022-09-27
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

# Mediator Pattern

중재자 패턴은 다양한 객체들의 Interaction, Communication을 Encapsulation하는 것을 목적으로 한다. 중재자 패턴은 기본적으로  상호작용에 참여하는 객체들의 통신에 관련된 것으로 이 통신의 책임을 한 개 객체에 위임하는 것을 뜻한다.

- Purpose
  - Allows loose coupling by encapsulating the way disparate sets of objects interact and communicate with each other.
- Use When
  - Commnunication between sets of objects is well defined and complex
  - Too many relationship exist and common point of control or communication is needed
- Advantages
  - Easy to understand the flow of communication
- Disadvantages
  - mediators are hardly ever reusable

## Mediator pattern in real-life

중재자 패턴은 공항에서 찾을 수 있다. 만약 비행기들끼리 서로 통신하며 착륙 순서와 활주로를 결정한다고 가정해보자. 한 두 개의 비행기들이 관여했을 때는 수월하게 진행되겠지만, 100대 이상의 비행기들이 수시로 드나드는 거대 공항에서는 혼란이 야기될 것이다. 이런 문제가 발생하는 것을 막기 위해 공항에는 관제탑이 존재한다. 이렇게 비행기들과 관제탑의 관계를 생각하면 중재자 패턴에서 중재자가 어떤 역할을 하는지 직관적으로 알 수 있을 것이다.

<img src="/_img/2022-09-27/mediator pattern airplane example.png">

### Class Diagram for Mediator

방금까지 살펴 본 비행기-관제탑 예제를 중재자 패턴을 고려한 다이어그램으로 표현하면 아래와 같이 나타낼 수 있다. 이렇게 나타내면 N개의 객체가 있을 때, N * (N - 1)개의 통신 링크를 N개로 줄일 수 있다. 

<img src="/_img/2022-09-27/airport example01.png">

이를 일반화하면 아래와 같이 나타낼 수 있다. 중재자 패턴은 아래와 같은 다이어그램으로 표현된다.

<img src="/_img/2022-09-27/mediator pattern.png">

### Object Diagram for Mediator

Mendiator Pattern은 Object Diagram을 보면 더욱 쉽게 이해 가능하다. 아래는 Mediator Pattern을 적용한 특정 상황에서의 Object Diagram을 표현한 것이다. 가장 중요한 것은 통신에 참여하는 모든 Obejct들은 서로 메시지를 주고 받는 것이 절대적으로 금지되어 있다는 것이다.

<img src="/_img/2022-09-27/mediator pattern object diagram.png">

## Structure and run-time mechanism of Mediator Pattern

Mediator Pattern을 적용하는 예로 FontDialog UI 구현을 들 수 있다. 각 객체가 본인의 변경 사실을 Mediator에게 전달하기만 하면, Mediator 내부 처리 과정을 통해 조건 부합하는 UI를 보여준다.

## Comparison with Observer Pattern

Observer Pattern에서는 Publisher 의 변경된 상태를 모든 Subscriber 에게 전달하는 것이 핵심이었다. 반대로 Mediator Pattern에서는 Colleague 상태가 변하면 Mediator가 이를 전달 받아 다른 참여자에게 전달하거나 특정한 기능을 수행한다. 중재자 패턴에서는 굳이 통신에 참여하는 모든 Colleague에게 변화를 공지하지 않아도 된다.

- Observer Pattern
    - Communication distributed by using observer and subject obejcts
    - reusable
    - can be hard to understand the multiple flows of communication
- Mediator Pattern
    - Mediator encapsulates the communication
    - mediators are hardly ever reusable
    - easy to understand the flow of communication