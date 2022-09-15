---
title: "[SW Engineering] UP - Elaboration (OOA / OOD / OOI)"
excerpt: "united process elaboration ooa"
date: 2022-09-15 18:00:00
layout: single
classes: wide
category:
    - software engineering
tag:
    - software engineering
author: 1FeS
comments: true
header:
    teaser: "/_img/thumb-nail/software-engineering.png"
---

# Elaboration - OOA

Elaboration Phase가 시작됐다는 것은 이 프로젝트를 진행하기로 결정했다는 것이다. 이는 실질적인 소프트웨어 개발의 시작 단계로서, 위험을 내포하고 있는 핵심을 구현 및 테스트하는 것에 초점을 맞춘다. 대부분의 요구사항들이 발견되고 안정화되며 가장 주요한 위험(Architecture risky and Valuable to customer)을 중점적으로 진행된다.

Elaboration Phase의 한 Iteration에 선택된 Use Case는 OOA, OOD 과정을 거쳐 개발된다. OOA에서는 기본적으로 Domain Model과 Use Case Model을 사용하는데, 여기서 Use Case Model이란 Use Case Diagram과 System Sequence Diagram을 합친 것을 말한다. OOD에서는 Design Model들로 구현하려는 동작이나 설계에 필요한 다이어그램을 사용한다. 끝으로 Elaboration Phase가 끝나면 Architecture가 정해지는데 이를 Software Architecture Document에 작성하게 된다. 이 과정이 잘 정리된 그림이 아래에 있다.

<img src="/_img/2022-09-15/relationship of up artifacts in one iteration.png">

## Domain Model

Domain Model은 클래스 다이어그램 개념을 사용해 실제 도메인의 객체를 개념화한 모식도이다. 많은 사람들이 Class Diagram의 스케치로서의 역할을 기억하지만, 클래스 다이어그램을 위한 Domain Model이 아니라 시스템 배경지식이 되는 Domain(분야)이라는 의미에서의 Domain을 이해하기 위해서도 작성된다. 이는 OOA 단계에서 가장 중요한 산출물로 객체 또는 클래스를 설계하기 위한 가장 기본적인 요소가 된다.

그래서 Domain Model은 해당 Domain Model이 Application을 구현하는데 그대로 사용되는 경우, Class Diagram의 이전 버전이라고 까지 말 할 수 있다. Elaboration Phase에서 추후에는 Domain Model 정보가 모두 Class Diagram에 녹아드는 경우도 있지만, 둘은 본질적으로 작성의 목적과 Operation이 없다는 점, Domain Model이 가진 빈약한 Association 측면에서 큰 차이를 보인다.

이러한 Domain Model의 목적은 앞서 설명한 것처럼 해당 분야와 시스템을 이해하기 위한 것이다. 그렇기 때문에 <u>옳다/그르다</u>를 구분하기는 어렵다. 그렇기 때문에 Correct 보다는 Useful 에 초점을 맞춰 **Understanding and Communication among a particular group** 을 달성하도록 작성하는 것이 중요하다.

## System Sequence Diagram

System Sequence Diagram은 System 수준에서 그리는 Sequence Diagram이라고 이해하면 된다. 중요한 것은 System을 Black-Box로 놓고 UML Sequence Diagram Notation을 차용해 그린다는 것이다.

그렇기 때문에 Obejct가 제공하는 Operation과 유사하게 시스템이 제공해야하는 Operation이 나타나는데 이를 System Operation이라고 한다. 그리고 이를 System이 구성하는 Object들의 Communication으로 Body를 만드는 것이 Object-Oriented Programming이며 System Sequence Diagram을 그리는 핵심이라 생각하면 된다.

Sequence Diagram에 관련된 내용은 [Sequence Diagram - 1FeS Study Note](https://wch18735.github.io/software%20engineering/SWEng_Sequence_Diagram/)에 잘 정리해 놓았다.

## Operation Contracts

Operation Contracts은 Optional 하며 대체 가능하지만 개념은 알아두어야 한다. 명심할 것은 필수적으로 작성하는 것은 아니라는 것이다. 

External Actor는 System이 제공하는 Interface만을 사용 할 수 있다. 그렇기 때문에 해당 System Interface을 계약서 또는 Acceptance 기준으로 사용 할 수 있다. 즉, 계약서처럼 활용 가능하다는 것이다.

계약서는 계약 당사자 어느 한 쪽이라도 불완전한 상태가 있다면 절대로 작성하지 않아야 한다. 그렇기에 보통 Elaboration Phase의 마지막 Iteration이 끝나고 나오는 완전한 System Operation을 종합해 Operation Contracts을 작성한다.

# Elaboration - OOD

Elaboration Phase의 각각의 Iteration마다 마치 Mini-waterfall처럼 Requirement Analysis → Design → Implementation → Test → Release 모든 과정을 수행한다. 그렇기 때문에 OOA에서 OOD로 넘어가는 과정은 필연적이며 이 때 중요하게 다뤄져야 할 내용이 바로 Architecture 결정이다.

## Logical Architecture and UML Package Diagrams

Software Architecture란 1999년 Booch, G., Rumbaugh, J. and Jacobson, I 이 The Unified Modeling Language User Guide 란 저서에서 다음과 같이 나타내었다. 아래 정보가 작성된 문서를 Architecture Document라고 한다.

> "A **software architecture** is the set of **significant decisions** about the organization of a software system, the <u>selection</u> of the structural elements and their interfaces by which the system is composed, together with their behavior as specified in the collaborations among those elements, the <u>composition</u> of these structural and behavioral elements into progressively larger subsystems, and the <u>architectural style</u> that guides this organization - these elements and their interfaces, their collborations, and their composition." 

우리에게 필요한 Logical Architecture는 UML Package Diagram 개념을 차용해 그려진다. 이는 OOA과정을 통해 정리한 클래스들을 패키지, 하위 시스템, 레이어로 논리적으로 묶어 표현하는 것을 말한다. 이런 Logical Architecture의 대표적인 예시로 Layered Architecture가 있다.

<img src="/_img/2022-09-15/layered architecture package diagram.png">

이렇게 Architecture가 작성되면 SSD(System Sequence Diagram)에서는 단순하게 표현되었던 내용이 어떻게 각 Layer로 전달되는지 표현해줄 수 있다. 아래에서 makeNewSale()은 단순하게 System을 호출하지만 우측의 Layered Architecture에서는 어떤 Layer를 통과해 전달되며 어떤 곳에서 구현되는지를 표현하고 있다.

<img src="/_img/2022-09-15/ssd and layer.png">

## Designing Objects: Satic vs. Dynamic

Object Model은 패키지, 클래스 이름, 속성과 메소드를 정의하도록 도와주는 Static model (ex: UML Class Diagram)과 논리적인 동작을 정의해 Method Body 작성에 도움을 주는 Dynamic model (UML Interaction Diagrams)로 나눌 수 있다. UP기반 Iterative 개발 방법론에서는 두 모델을 번갈아가며 작성하는 작업을 반복해 상호보완적으로 만들기 때문에 어느 한 쪽이 중요하다고 말할 수 없다.

Static UML Tools은 Class Diagram, Package Diagram, Deployment Diagram 세 가지가 있다. 반대로 Dynamic UML Tools는 Interaction Diagrams, Statechart Diagram, Activity Diagram 등이 있다.

## Object-Oriented Design Skill over UML Notation Skill

UML을 잘 사용하는 것은 Object Design을 위한 도구일 뿐이다. Object Design을 위해서는 Principles of responsibility assignment(GRASP), 23 Design Patterns of GoF(Gang of Four) 등과 같은 지식을 익혀야 한다. 물론 UML을 잘 익혀놓으면 UML로 모식화된 패턴들을 보다 쉽게 익힐 수 있기 때문에 소홀히 다뤄도 된다는 것은 아니다.

# Elaboration - OOI

Implementation 단계에서는 Object끼리 어떻게 상호작용 하는지를 기반으로 Development에 필요한 내용을 채워나간다.

## Visibility Between Objects

Visibility란 특정 Object에서 다른 Object의 Operation이 보이는지 여부라고 이해하면 편하다. 예를 들어 임의의 Object가 가진 Operation이 Visible하다면 호출하여 사용 가능하다. 앞선 예시를 보충 설명하는 예제가 Sequence Diagram의 Messeage Sender / Receiver 이다. Sequence Diagram에서 Messeage Sender가 호출하는 Operation은 Receiver에 반드시 정의되어 있어야 하며 Sender에게 Visible하다고 표현할 수 있다.

Visibility는 Attribute, Parameter, Local, Global 수준의 Visibility가 있다. Attribute Visibility는 두 Object가 삭제되지 않고 존재하는 이상 유지되기 때문에 상대적으로 Permanent하다고 얘기한다. 반면 Parameter Visibility는 사용하는 Operation에 전달되어 동작하고나면 이후 해당 Parameter에 접근할 수 없기 때문에 Temporary하다고 표현하며 Dependency를 동반하는 경우가 있다.

이 때, Parameter는 Attribute로 쉽게 전환 가능하다. 예를 들어 전달받는 매개변수를 Assignment 후 사용하면 Prameter Visibility가 Attribute Visibility로 바뀌게 된다. 마찬가지로 코드 레벨에서 Local Visibility를 설명하자면 Local Visibility는 **Create a new local instance** 또는 **Assign the returning object from a mothod invocation to a local variable** 두 가지 일반적인 방법으로 만들어지는데, Method 안에 new 를 통해 생성하거나 return 받아 사용하는 모든 것을 뜻함을 알 수 있다.

끝으로 Global Visibility는 Global로 선언해 Permanent한 특징을 가지는 Visibility인데, OO 개념에서는 Global 개념이 없기 때문에 Singleton Pattern을 통해 구현하게 된다.

## Mapping Designs to Code

Object-Oriented 진영에서는 구현을 Code에 Mapping한다고 표현한다. 이는 작성된 문서를 바탕으로 엔지니어의 개인적 주관이 최대한 배제하여 기계적이고 체계적으로 작성한다는 뜻을 내포하고 있다. 가령, 작성된 Class Diagram으로부터 Skeleton Code를 만들어 내고 Sequence Diagram을 통해 Body를 채울 수 있다. 

Implementation 순서는 **least-coupled to most-coupled** 를 원칙으로 한다. 가장 Dependency가 없는 클래스부터 작성해 나가는 것이며 아래는 Dependency에 따른 순서를 나타낸 예제다.

<img src="/_img/2022-09-15/order of implementation.png">