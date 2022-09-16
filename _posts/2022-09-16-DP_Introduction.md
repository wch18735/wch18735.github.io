---
title: "[Design Pattern] Introduction"
excerpt: "design pattern"
date: 2022-09-16
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

# Design Pattern

패턴이란 자주 반복되는 문제를 해결하기 위한 방법이라 생각하면 되겠다. 사실 이 개념이 처음 등장한 곳은 소프트웨어 진영이 아닌 건축 진영이었다. 건축가 Christopher Alexander는 건축물과 도심을 설계하는 과정에서 비슷한 유형의 문제가 빈번하게 발생하는 것을 느끼게 되었다. 이렇게 비슷한 유형의 문제들은 해결하는 방법도 비슷했는데 이들을 모아 출간한 것이 A Pattern Language라는 책이었고, 이후 Software 진영에서 해당 언어를 차용해 사용하기 시작했다.

실제로 건축과 소프트웨어 개발은 비슷한 느낌을 가진다. 건축 과정에서도 설계자가 설계를 마치면 안전 관리자와 인부들이 투입되어 건물이 올라간다. 소프트웨어 역시 설계자가 있고 프로젝트 관리자와 SI 인력이 투입되어 산출물이 만들어져 간다. 이렇게 정리하다보니 그 속에 많은 이해 관계자가 얽혀있는 것부터 대책없는 예산 감축과 납기 일정으로 인해 고생하는 현장직까지 새삼 닮은 점이 많다고 느껴진다.

아래에 소프트웨어 패턴 역사 속 굵직한 내용들을 간략히 정리했다.

- 1977: The architect Christopher Alexander, A Pattern Language: Towns, Buildings, Construction
- 1987: Kent Beck and Ward Cunningham, OOPSLA Paper: Adopted Alexander's pattern idea for Smalltalk GUI design
- 1995: Gang of Four (Gamma, Hel, Johnson, Vissides) Design Patterns: Elements of Reusable Object-Oriented Software
- ~ Present: Pattern Languages of Programs (PLoP) Conferences and Books

## Why Pattern

Gang of Four 멤버 중 한 명인 Erich Gamma는 객체지향 소프트웨어 설계는 매우 어려우며, 재사용 가능한 설계는 더욱 어렵다고 얘기한다. 그렇기 때문에 막무가내로 개발하는 대신, 우리는 기존에 잘 설계되었던 패턴들을 익혀 유연하고 재사용 가능한 소프트웨어를 설계하는데 한 걸음 더 다가갈 수 있다.

## Category of GoF Patterns

GoF Design Pattern에서는 23가지 패턴의 종류를 크게 세 가지로 나누고 있다. 원문을 짧게 번역하자면 **Creational Pattern**은 객체를 생성하는데 관련된 패턴들로 객체가 생성되는 과정의 유연성을 높이고 동작과 생성을 분리하도록 지도한다. **Structural Pattern**은 소프트웨어 구조에 관련된 패턴들로 객체지향에서 사용하는 상속 등을 사용해 클래스나 객체를 구조화한다. 끝으로 **Behavioral Pattern**은 반복적으로 사용되는 객체들의 상호작용을 묘사한 것으로 객체들간의 책임을 효율적으로 분리하는 다양한 방안들을 제공한다.

- Creational
    - Address problems of creating an object in a flexible way
    - Seperate creation from operation/use
- Structural
    - Address problems of using OO cunstructs like inheritance to organize classes and objects
- Behavioral
    - Address problems of assigning responsibilities to classes
    - Suggest both static relationship and patterns of communication

앞으로 정리하게 될 패턴들은 적용 범위와 목적에 따라 아래와 같이 분류 할 수 있다.

<img src="/_img/2022-09-16/gof_types.png">

## Levels of Patterns [POSA]

패턴은 비단 디자인 패턴만이 아니라 다양한 종류가 존재한다. 문제를 해결하기 위해 각각의 패턴이 적용될 수준을 살펴본 후 특징을 고려해 적재적소에 적용한다면 유연하고 재사용 가능한 소프트웨어를 작성할 수 있다.

- Architectural Pattern
    - is funcamental structural organization or schema
    - provides predefined subsystems, specifies their responsibilities, and includes rules and guidelines for organizing relationships between them
    - affects the overall skeletal structure and organization of a software

- Design Pattern
    - refine subsystems or components, or relationships between them
    - describes commonly recurring structure of components that solves a general design problem within a particular context
    - does not influence overall system structure, but instead define micro-architectures of subsystems and components

- Coding Pattern (or Programming Idiom, Convention)
    - is low-level pattern specific to a programming language
    - describes how to implement particular aspects of components or the relationships between them using the features of the given language

## 변하는 것과 변하지 않는 것의 분리

디자인 패턴을 다시 공부하며 올해 초 사내 강의로 들었던 **C++ 기반 Design Pattern & Refactoring** 교재를 펼쳐봤다. 결국 디자인 패턴의 핵심은 <u>변하는 것과 변하지 않는 것을 분리</u>라는 필기가 있다. 그리고 변하는 부분을 분리하는 방법은 크게 **가상함수로 분리**하는 방법과 **다른 클래스로 분리**하는 방법이 있다고 한다.

클래스 기반 분리는 실행 시간에 정책을 변경 가능하게 만들고, 제공하는 인터페이스를 통해 정책을 사용하는 모든 클래스에서 정책 재사용을 가능하게 만드는 장점이 있다고 한다. 가령, 클래스들과 하위 클래스 간의 관계를 다루는 Class Pattern은 컴파일 시에 관계가 결정되나 객체 간의 관계를 다루며 실행 시간에 관계가 생성되는 Object Pattern의 차이라 이해 할 수 있다.