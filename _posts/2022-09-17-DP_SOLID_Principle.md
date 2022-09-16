---
title: "[Design Pattern] SOLID Principle"
excerpt: "solid principle"
date: 2022-09-17
layout: single
classes: wide
category:
    - design pattern
tag:
    - software engineering
author: 1FeS
comments: true
header:
    teaser: "/_img/thumb-nail/software-engineering.png"
---

# SOLID Principle

## Hierachy of Pattern Knoledge

아래는 Strategy Pattern을 예시로 패턴 지식의 계층 관계를 표현한 그림이다.

<img src="/_img/2022-09-17/design basics.png">

## Design Smells

Design Smells란 잘못 된 설계에서 발생하는 증상이나 신호를 말한다. 이런 신호는 경직성, 취약성, 부동성, 점착성, 불필요한 복잡성, 불필요한 반복, 불투명성 등으로 분류할 수 있으며 그 설명을 아래에 정리했다.

- **Rigidity:** The system is hard to change, because every time you change one thing, you have to change something else in a never ending succession of changes.
- **Fragility:** A change to one part of the system causes it to break in many other, completely unrelated parts.
- **Immobility:** It is hard to disentangle the system into componens that can be reused in other systems
- **Viscosity:** If it is easier to add a hack than it is to add code that fits into the design, then the system has high viscocity
- **Needless Complexity:** "There are lots of very clever code structures that aren't actually necessary right now, but could be very useful one day"
- **Needless Repetition:** "The code looks like it was written by two programmers"
- **Opacity:** Elucidation of the originator's intent presents certain difficulties related to convolution of expression

이런 설계적 결함이 나타나는 이유는 근본적으로 Dependency 관리가 되지 않기 때문이다. 그렇기 때문에 우리는 보다 나은 객체지향 설계를 위해 객체지향 설계 사상과 이를 잘 수행하기 위한 원칙을 알아야 한다.

## R.C. Martin's Software Design Principle - SOLID

R.C. Martin's Software design principles는 각 원칙의 앞 글자를 따 SOLID 원칙이라고도 불린다. 

- SRP: Single-Responsibility Principle
- OCP: Open-Close Principle
- LSP: Liskov Substitution Principle
- ISP: Interface Segregation Principle
- DIP: Dependency Inversion Principle

### SRP: Single Responsibility Principle

SRP는 하나의 클래스는 하나의 책임만 지도록 만드는 것을 뜻한다. 다르게 말하면 클래스가 변화해야 할 이유는 단 하나여야 한다는 뜻이다. 만약 한 개 클래스가 하나 이상의 책임을 가지고 있으면 이는 결합(Coupled)을 불러와 수정에 어려움을 가져오게 된다.

여기서 책임이란 클래스가 이행해야하는 계약, 의무, 해야하는 일이라고 표현한다. 그러나 한 클래스에 많은 책임이 부여되면 추후 수정에 오류가 발생할 가능성이 높아진다. 이러한 SRP와 관련이 깊은 Measure는 다양한 책임들이 얼마나 강하게 연관되고 집중되어 있는지를 측정하는 Cohesion이다.

> Cohesion: How strongly-related and focused are the various responsibilities of a module

아래는 SRP를 위반한 한 가지 사례를 가져온 것이다. 좌측과 같은 클래스 다이어그램에서 AClient가 학생 정보를 SSN, Name 기준으로 정렬된 형태로 받아보기 위해 Comparable 인터페이스를 구현해 compareTo()를 사용하고자 하는 모습이다.

<img src="/_img/2022-09-17/srp violation 01.png">

우측과 같은 형태에서 정렬 기준이 변경되어 compareTo() 구현을 수정하는 경우 Target이 변경되기 때문에 Source에 해당하는 Register 클래스도 아무 연관이 없지만 영향을 받을 수 있다. 이와 같은 경우 SRP 위반을 해결하는 방향으로 다시 설계할 수 있다. 바로 책임을 나누는 것이다.

변경될 수 있는 Comparable 인터페이스를 상속하는 SortStudentBySSN, SortStudentByName 클래스를 만들어준다. 이렇게 되면 정렬 기준이 변경되거나 추가되더라도 Register 클래스는 변경의 영향에서 벗어나게 된다.

그러나 SRP를 고려한 설계를 잘 하기 위해서 미래에 변화될 수 있는 부분을 모두 분리하고자 한다면 이는 오히려 오버헤드가 된다. 그렇기 때문에 Application의 진화 방향을 잘 생각해보고 영속적인 기능들을 적절하게 나눌 필요가 있다. 짧게 말해 Needless Complexity를 피하는 방향의 설계가 필요하다.

<img src="/_img/2022-09-17/srp violation 02.png">

### OCP: Open-Close Principle

OCP는 기능 확장에는 열려있고, 코드 수정에는 닫혀있어야 한다는 뜻이다. 우리가 가장 처음 객체지향 특징 중 Polymorphism에 대해 배울 때, **if-else** 분기를 대체하는 예제를 다룬다. 이것이 대표적인 OCP를 따르는 설계 방식으로 기반 클래스를 상속하는 클래스들에 대하 변경에는 열려있으며, 그에 따른 변경에도 클라이언트 코드 수준에서의 불필요한 수정을 예방한다.

예를 들어 Employee 클래스에 Attribute 관리부터 DB Access 책임까지 모든 기능을 집어 넣은 경우, 한 개의 기능 수정이 다른 곳에 지속적으로 영향을 끼치게 될 가능성이 생긴다. 또한, 책임 이외에도 아래와 같은 방식으로 구현한 코드는 **if-else** 분기를 사용해 동작을 수행하는데, 이는 Design Pattern Smells의 Rigidity를 보여주는 대표적인 예시다.

<img src="/_img/2022-09-17/ocp violation 01.png">

이를 해결하기 위해 앞서 말한대로 Polymorphism을 사용한다. 아래와 같은 디자인은 새로운 고용 형태에 대해서도 incAll 메소드를 수정하지 않고도 본인의 월급을 높이는 동작 수행이 가능하다.

<img src="/_img/2022-09-17/ocp violation 02.png">

이런 OCP를 만족하는 설계는 보통 Polymorphism을 사용하는데 이는 동적 바인등을 요하는 동작이기 때문에 Runtime에 조금 더 많은 자원을 소비하게 된다. 그렇기 때문에 모든 곳에 적용하기는 어려울 수 있어 첫 설계에는 최대한 가시적인 부분을 처리하고 Test Driven Development 사상을 통해 수정이 필요한 상태가 발생하는 순간 처리하도록 가이드한다.

> "Fool me once, shame on you. Fool me twice, shame on me"

### LSP: Liscov Substitution Principle

LSP는 Liscov Substitution Principle로 **Subtype must be substitutable for their base types**라는 뜻을 가지고 있다. 이를 구현 관점에서 설명하면 파생 클래스로 기반 클래스를 대체할 수 있어야 함을 뜻하는데, 만약 P의 서브타입인 C가 있을 때, P가 사용되고 있는 부분을 C로 대체했을 때 문제가 없다는 것이다. 이런 기준을 적용해 상속을 사용해야 하는지 여부를 결정할 근거로 사용되기도 한다.

여기서 Subtyping과 Implementaion Inheritance와는 조금 차이가 있다. Subtyping은 IS_A 관계를 설정하는 **interface inheritance**를 뜻한다. 그러나 Implementation Inheritance는 구현을 재사용하고 Syntatic Relationship을 설정하는 것일 뿐 Semantic Relationship을 필수적으로 설정하지는 않는 **code inheritance**를 뜻한다. 그러나 대부분의 OOP Language는 Subtyping과 Implementation Inheritance를 동시에 수행해버리기 때문에 구분이 어렵다. 

아래 그림은 LSP를 적용해 발생한 문제를 해결하는 경우이다. 새로운 개발자 A는 LSP가 고려되었을 것이라 생각해 List가 사용된 모든 곳에 Queue를 집어 넣었다. Queue는 FIFO 성질을 만족해야 하지만 List 동작을 모두 상속했기 때문에 이를 대체할 수 있었고, push_front()나 pop_back()과 같은 작업을 수행할 수 있었다. 이는 명백히 잘못 된 것이다.

이를 올바르게 바꾸기 위해 List를 대체할 수 있는 상속대신 Aggregation이나 Composition을 사용해 List Interface를 내부에 담고 사용하는 방법을 나타내고 있다. 한 가지 주의할 점으로 LSP를 만족시키기 위해 상속 관계를 정리하는 경우 OCP를 만족하지 못 하여 컴파일 에러가 발생하는 경우가 생길 수 있다.

<img src="/_img/2022-09-17/lsp violation 01.png">

### ISP: Interface-Segregation Principle

ISP는 인터페이스 격리 또는 분리 원칙으로 커다란 인터페이스보다 기능별로 분리된 인터페이스가 좋다는 뜻이다. 클라이언트는 그들이 사용하지 않는 메소드에 의존하도록 강제되면 안 된다. 예를 들어 특정한 Interface에 많은 기능들이 묶여 내가 사용하지 않는 것까지 포함된 Fat Interface가 제공되는 경우가 있는데, 이를 잘게 쪼개서 사용할 수 있다면 나누는게 좋다.

<img src="/_img/2022-09-17/isp violation 01.png">

### DIP: Dependency Inversion Principle

DIP는 의존 관계 역전을 뜻하며 이는 높은 수준의 모듈은 낮은 수준의 모듈에 의존해서는 안 되며, 각각은 모두 추상화에 의존해야 한다는 것이다. 이를 계층적 관점에서 설명하자면 추상화 계층은 디테일 계층에 의존하지 않지만 디테일한 구현 계층은 반드시 추상화 계층에 의존해야 한다는 것을 뜻한다.

각각의 단어를 조금 더 자세히 설명해보려고 한다. 앞서 언급되었던 높은 수준(High-level resources)의 모듈은 Interface와 Abstract Class를 말하며 낮은 수준(Low-level resources) 모듈은 Concrete class를 말한다. 또한, 여기서 역전이라는 단어는 과거 구조적 분석 및 설계를 진행했을 때 나타나는 모듈 다이어그램의 의존 관계를 뒤바꾼다는 의미에서 사용되었다.
