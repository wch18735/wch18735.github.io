---
title: "[SW Engineering] SASD (Structural Analysis Structural Design)"
excerpt: "structural analysis and structural design"
date: 2022-02-24
layout: single
classes: wide
category:
    - software engineering
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

# Structured Anlysis

구조적 분석이란 시스템, 요구 사항들을 모델링해 분석하는 작업이라고 생각하면 편할 것 같다. 여기서 모델링의 대상이 되는 모델이란 추상화할 대상을 이해하기 쉽게 모식도화 하는 것이라 할 수 있다. 즉, 구조적 분석은 시각화 도구와 관련 기술을 기반으로 개발하려는 소프트웨어 명세를 이해하기 쉽게 분석하는 것을 뜻한다. 

나는 구조적 분석을 개발하려는 소프트웨어를 다양한 관점에서 **모델링** 하는 것으로 이해했다. 예를 들어, 소프트웨어의 데이터나 기능을 모델링하면 DFD, ERD 라는 산출물이 나타난다는 것처럼 특정한 모델링 방법을 뜻한다 생각한다.

물론 효과적인 분석을 위해서는 비단 구조적 분석뿐만 아니라 목적에 따라 다양한 분석 방법을 적용할 수 있을 것이다. 각자의 경험들을 나열하거나, 브레인 스토밍을 하거나, 마인드맵을 그리는 것들도 분석 기법 중 하나로 사용되는 것처럼 구조적 분석은 사용되는 수 많은 방법들 중 하나의 방법일 뿐이다. 그렇기 때문에 소프트웨어 설계를 위한 방법으로 언제나 강박적으로 구조적 분석 방법론을 들이밀지 않는 것이 중요하다. 

## History of SASD (Structured Analysis and Structured Design)

SASD는 구조적 분석 및 설계의 약자로, 절차지향적 시스템 개발을 위한 요구사항 분석 과정을 뜻한다. 반대로 객체지향적 시스템 분석은 OOAD 를 따라 진행된다. 절차지향적 SA 특징으로는 큰 범위에서 작은 범위로, 복잡한 구조에서 단순화된 것으로 잘게 쪼개는 방식인 Top-Down Divide and Conquer 접근법으로 수행한다는 것이 있는데, 자세한 순서는 아래와 같다.

- Statement of Purpose
    - 개발해야 할 소프트웨어의 명확하고 깔끔한 문장
    - PFR(Preliminary Functional Requirements) 작성
- System Context Diagram 작성
    - DFD Level 0
- Event List 작성
- Data Flow Diagram 작성
    - Hiaracheically 작성
    - 발생하는 데이터 명세를 위한 Data Dictionary 작성
- ERD 작성
- 완성된 Model을 SRS Template 기준으로 작성

# Architectural Design

이전 단계에서 요구사항 분석이 끝났다면, 이제 해당 요구사항에 맞춘 구조적인 설계가 필요하다. `~하게 동작한다` 라는 명세들이 만들어졌다고 곧바로 개발을 시작할 수는 없다. Architectural Design(구조적 설계)은 개발 단계 이전에 시스템이 어떻게 조직되어야 하는지, 어떻게 설계해야하는지에 관한 내용을 다룬다. 즉, Architectural Design 은 요구 공학 산출물과 설계 및 구현 사이를 잇는 역할을 수행하며 시스템의 주요 컴포넌트들과 그 관계들을 정의한다.

이러한 Architectural Abstraction 은 하드웨어 자원 설치나 개발 패턴과 같은 큰 개념에서 프로젝트 소스코드 구조는 어떻게 관리하며 클래스들간의 관계는 어떠한지 등의 작은 영역까지 모든 영역에 걸쳐 이뤄질 수 있다. 물론 복잡한 구조의 상업 시스템이나 규모가 작은 프로그램에까지 모두 적용 가능하다. 

이렇게 구조적 설계 과정에서 추상화를 수행하면 이해 관계자와의 커뮤니케이션을 원활히 수행할 수 있으며, 구축할 시스템을 세부적으로 분석할 수 있으며, 재사용성을 확보할 수 있다는 장점을 얻을 수 있다.

## Architectural Models

모델이란 추상화할 대상을 이해하기 쉽게 모식도화 하는 것이라 설명했었다. 구조적 모델은 `block diagram`, `box diagram`, `line diagram`, `extension of UML model` 등으로 나타내질 수 있다. 그리고 이렇게 나타내진 구조 모델은 High-level architectural view 를 제공한다. 이해 관계자들이 원하고 신경쓰는 이른바 **Concerns** 에 대하여, 각각에 대한 Architectural Description 을 제공해 요구 조건을 충족시킬 수 있는 기반을 마련하는 것이다.

표준 QA 요소 중 하나인 Security 가 Customer 의 Concern 중 하나라고 해보자. 설계자는 Layered Architecture 로 시스템을 설계해 보안적 요소를 강화할 수 있고, 이를 블록 다이어그램이나 UML 다이어그램을 통해 시각화할 수 있다. 해당 자료는 PPT에도 쓰일 수 있고, 타당성 검증에도 사용될 수 있고, 추후 유지 및 보수 관리가 필요한 순간에도 시스템 이해를 돕는 중요한 자료가 될 것이다.

## Architectural Design Decisions

구조적 설계 결정 과정은 창의성을 요구한다. 시스템이 어떻게 구현되고, 개발될 것인지와 주어진 환경과 자원에 따라 다양한 형태가 나타날 수 있다. 당연히 중점적으로 다루는 시스템 특징에 따라 Stand-alone, Client-Server, 3-Tier Architecture 등 여러 가지를 고려할 수 있다. 그러나 기본적으로 `Performance`, `Security`, `Safety`, `Availability`, `Maintainability` 은 공통적으로 검토해봐야 할 시스템 특징들로 다뤄지고 있다.

## Architectural View

`4 + 1 View Mode of Software Architecture`는 `Logical`, `Process`, `Development`, `Physical` 네 가지에 더해 `Use-Case Scnario` 까지 네 가지 관점에서 시스템 구조를 확인하는 것을 뜻한다. 소위 투자프로세스를 태운다고 말할 때 작성하는 문서들이 위 관점들에서 제안서를 검토하는 느낌이 든다. 

일례로 Physical View 는 시스템의 물리적인 하드웨어가 소프트웨어를 구현하는데 어떻게 분산될 것인지를 말한다. Web Application 으로 따지면 서버는 어느 지역에 두고, 어떤 망 내부에 위치시켜야 하는지 등을 결정하는 것이다. 이를 Architectural Patten 이라고 한다.

## Architectural Patterns

구조적 패턴은 아래와 같다. 앞서 구조적 설계를 수행할 때, 공통적으로 고려되어야 할 몇 가지 속성이 있다고 했다. 공통적으로 포함되어야 할 속성들이 있다면 자연히 비슷한 형태의 패턴들이 생기게 되는데, 이를 아래와 같이 나타낼 수 있다. 

- MVC
- Layered
- Repository
- Client-Server
- Pipe & Filter
- etc

나는 컴공이나 소프트웨어 전공이 아니라 각각을 띄엄띄엄 혼자 배워 그저 웹 개발에만 쓰이는 것으로 알고 있었는데, 이렇게 더 넓은 개념으로 알게되니 새로웠다.

자세한 내용을 더 알고 싶다면 `Pattern Oriented Software Architecture vol.1 ~ 5` 를 읽어보자. 그리고 전반적인 Software Engineering 개론을 익히고 난 뒤, 천천히 읽어볼만한 저서로 `Software Engineering at Google` 을 읽어보라는 얘기를 어딘가에서 들었다. 

이후 개략적인 구조를 잡은 후 시작되는 것이 Design and Implementation 이다. 본격적인 개발 단계다. 지금까지는 이런 과정 없이 무작정 개발했던 경험이 절대적이었으니 체계적으로 시스템을 구조화한 뒤 설계부터 개발까지 진행해보며 그 차이를 느껴보는 것도 좋은 경험이 될 것 같다.

## Application Architecture

Architectural Pattern 은 시스템 자원, 데이터 등을 어떻게 배치할 것인지 등을 나타낸다. Application Architecture 과는 다르게 시스템이 어떤 형태의 요구사항을 가지는가를 표현한다. 

수학 문제로 비유하자면 어떤 문제는 도형 문제고, 어떤 문제는 다항식 문제다 같은 형태로 나누는 것이다. 다만 현대 비즈니스 시스템들은 대부분 공통적인 요구사항들을 내포하고 있기 때문에 비슷한 유형으로 굳어지는 것 같다.

- Data Processing Applications
- Transaction Processing Application
- Event Processing Systems
- Language Processing Systems
- etc..

예를 들어, DBMS를 설계해야 한다면 Transaction Proccsing Application 유형으로 분류할 수 있으며, 해당 유형의 소프트웨어는 어떤 특징들이 있는지, 어떤 것들을 유의해야 하는지 관련 정보를 알 수 있다. 

물론 개발하려는 소프트웨어 또는 어플리케이션이 어떤 역할을 하는지나 성격을 갖는지에 따라 다양한 특징을 포함할 수 있다. 또한 소프트웨어 개발에 포함되는 다양한 분야 중 어떤 파트를 담당하는지에 따라 주의할 영역이 달리지니, 속단하지 않는 것이 중요하다.