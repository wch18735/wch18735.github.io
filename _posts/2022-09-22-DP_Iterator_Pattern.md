---
title: "[Design Pattern] Iterator Pattern (반복자 패턴)"
excerpt: "iterator pattern"
date: 2022-09-22
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

# Iterator Pattern

Iterator는 Cursor라는 다른 이름을 가지고 있다. 이는 Aggregate Object 속 원소들이 어떤 구조로 모여있는지 알 수 없어도 그들을 지칭해 사용 할 수 있도록 지시자를 제공한다.

> An aggregate object is an object that contains other objects for the purpose of grouping those objects as a unit. It is also called a container or a collection.

아래에는 Iterator Pattern의 역할, 목적, 사용 조건을 간략하게 정리했다.

- Iterator Pattern
    - Provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation
- Purpose
    - Allows for access to the elements of an aggregate object without exposing its underlying representation
- Use When
    - Access to elements is needed without access to the entire representation
    - Multiple or concurrent traversals of the elements are needed
    - A uniform interface for traversal is needed
    - Subtle difference exist between the implementation details of various iterators

## The Objectvile Diner and Pancake House

Iterator Pattern을 적용하는 예시로 Head First Design Pattern에 실린 The Objectvile Diner and Pancake House를 들어보려고 한다.

초기에는 아래와 같은 형태로 Menu가 존재한다. 각 메뉴마다 메뉴 정보를 추가하는 메소드가 정의되어 있다. 이제 이 두 메뉴를 같은 가게에서 관리하게 되어 순회하는 방법을 통일해야 하는 상황이라고 가정하자.

<img src="/_img/2022-09-22/objectvile 01.png">

이들이 가진 menuItems를 순회하려면 각각 index 접근과 next 접근을 수행해야 한다. 즉, 순회 방법이 달라지게 된다. 이와 같이 집합을 이루는 다양한 종류의 방법이 있기 때문에 순회 방법의 일반화를 위한 Iterator(순회자)를 사용한다. Iterator를 사용한 다이어그램으로 나타내면 아래와 같다.

<img src="/_img/2022-09-22/objectvile 02.png">

앞서 제공된 다이어그램에서는 Waitress 클래스가 PancakeHouseMenu와 DinerMenu를 모두 사용하고 있었다. 그러나 이는 메뉴가 추가될 때마다 코드가 수정되어야 하는 번거로움이 생길 수 있는 구조이다. 따라서 Iterator를 사용함에 있어 **순회**라는 기능적 개념에만 조금 더 초점을 맞추는 방향으로 구조를 개선해 볼 수 있다. 

> ※ Java에서 Iterator Pattern 적용을 지원하는 인터페이스 **java.util.Iterator**를 사용하면 더 쉽게 설계가 가능하며, List Collection은 ListIterator Interface가 존재하여 조금 더 다양한 기능을 제공해준다.

<img src="/_img/2022-09-22/objectvile 03.png">

## Structure of Iterator pattern

지금까지 보았던 예제를 일반화하면 아래와 같이 나타낼 수 있다. Client에게 Aggregate이 어떤 자료구조를 가지고 있는지 모르도록 정보를 숨기기 위해 Iterator Interface를 사용한다.

<img src="/_img/2022-09-22/iterator pattern structure.png">

이러한 Iterator Pattern과 관련 깊은 원칙이 Single Responsibility Principle이다. 이 패턴을 사용함으로써 Aggregate Reponsibility와 Iteration Responsibility를 나눌 수 있게 해준다. 이를 만족하면 자연스레 높은 Cohesion을 갖춘 설계가 될 수 있다.

## Related Patterns

- Iterator can traverse a Composite. Visitor can apply an operation over a Composite
- Polymorphic Iterators rely on Factory Methods o instantiate the appropriate Iterator subclass
- Memento is often used in conjunction with Iterator. An Iterator can use a Memento to capture the state of an iteration. The Iterator stores the Memento iternally