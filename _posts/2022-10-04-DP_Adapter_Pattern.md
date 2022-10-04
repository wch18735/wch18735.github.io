---
title: "[Design Pattern] Adapter Pattern (어댑터 패턴)"
excerpt: "adapter pattern"
date: 2022-10-04
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

# Adapter Pattern

Adapter는 Wrapper라고도 알려져있다. Adapter의 목적은 실제 세계에서의 Adapter와 마찬가지로 서로 다른 인터페이스를 가진 클래스들이 동일한 동작을 수행하도록 도와주는 것이다. 실제로 서로 다른 개발자들에 의해 구축된 라이브러리들은 종종 서로 다른 인터페이스를 제공한다. 이를 동일한 형태로 사용하기 위해 어댑터 패턴을 사용하게 된다.

> Adapter pattern solves a incompatiblity of interfaces

어댑터를 만드는 매커니즘은 다음과 같다. 먼저 클라이언트는 특정한 동작을 기대하고 어댑터에게 명령을 내린다. 그러면 어댑터는 클라이언트가 실제로 사용하려는 대상인 Target Interface(Adaptee)를 호출한다. 이 과정에서 어댑터는 클라이언트의 요청을 **해석**하여 Target Interface를 호출하게 된다. 끝으로 Target Interface의 요청에 대한 응답을 전달받아 클라이언트에게 반환해주는 것까지가 어댑터의 역할이 된다.

<img src="/_img/2022-10-04/adapter example.png">

## Object Adapter vs Class Adapter

Adapter Pattern은 두 가지 방법으로 구현할 수 있다. 먼저 Object Adapter이다. Object Adapter는 Object Composition과 Delegation을 이용해 어댑터를 구현한다. 이는 여러 Target Interface를 구현하는 다기능 어댑터를 구현하는데 사용될 수 있다.

<img src="/_img/2022-10-04/object adapter.png">

다른 하나는 Inheritance를 이용해 구현하는 Class Adapter이다. 이 때는 객체간의 관계가 아니라 클래스끼리의 관계를 통해 구현된다. 구현이 간단하며 Adaptee에 존재하는 다양한 정보를 사용할 수 있다.

<img src="/_img/2022-10-04/class adapter.png">

## Implementation Example and Isues

어댑터 패턴을 공부할 때 가장 많이 언급되는 예시는 Iterator와 Enumeration이다. 이전 버전의 Java collection, 예컨데 Vector, Stack, Hash Table 등의 자료를 참조할 때는 hasMoreElements(), nextElement() 메소드를 사용했다. 그러나 최근 Java collection은 Iterator를 사용하며 이들은 hasNext(), next(), remove() 등의 메소드를 사용한다.

즉, 아래와 같이 우리는 Iterator를 사용하려고 하지만 Library는 Enumeration으로 구현된 것들이 있다는 것이다.

<img src="/_img/2022-10-04/java collection example.png">

어댑터 구현은 이름만 다르거나 전달되는 매개변수 순서만 다른 아주 간단한 단계부터 전반적인 동작 과정이 상이한 메소드의 병합에 까지 다양한 난이도가 존재한다. 또한, 한 개 방향으로만 존재하는 어댑터에서 양방향 어댑터, 다방향 어댑터까지 고려해 볼 수 있기 때문에 주어진 문제에 따라 깊은 고민을 요구하는 경우도 있다.

