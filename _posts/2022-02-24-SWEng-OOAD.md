---
title: "[SW Engineering] OOAD (Object Oriented Analysis and Design)"
excerpt: "design and implementation for object-oriented system"
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

# Object-Oriented Analysis and Design

소프트웨어 설계를 위해서는 다양한 설계 프로세스가 있지만, 객체 지향 시스템을 설계하기 위한 프로세스는 대부분 OOAD 를 따른다. 물론 OOAD 가 다른 설계 프로세스와 크게 차이나는 것은 아니다. 객체 지향 설계 프로세스는 대부분 공통적인 과정을 거친다.

- Define the context and modes of use of the system
- Desing the system architecture
- Identify the principal system object
- Develop design model
- Specify object interface

OOAD는 OOA(Object Oriented Analysis)와 OOD(Objec Oriented Design) 두 단계로 구분된다. 즉, 분석을 수행한 후 분석 결과에 따라 설계를 수행하게 된다. 개발은 분석과 설계가 모두 끝난 후 진행한다는 것을 명심해야 한다. 그렇기 때문에 개발자에서 엔지니어가 되기 위해서는 단순 기술 적용 단계를 넘어 더 높은 수준의 분석, 설계 역량을 갖춰 나가는 것을 목표로 해야한다.

OOA 단계에서는 도메인의 지식을 바탕으로 Object를 도출하는데, 이 때 사용되는 것이 Requirement Specification 또는 Use Case 이다. 이렇게 시스템에서 공통적으로 사용될 Object들을 나타내기 위해 Domain Model을 사용하게 된다. OOD 단계에서는 찾아진 Domin Model들을 특정 시나리오인 Sequence Diagram에 맞게 정의하는 과정을 가진다. 이 단계를 거치고나면 Domain Model에 Operation과 Attribute를 추가하고, 관계가 추가된 Class Diagram이 만들어진다.

## UML for OOAD

UML(Unified Modeling Language)는 시스템 구조를 도식화하고, 구축하고, 특정하기 위해 사용되는 시각적 언어이다. 이 UML은 사용자에 따라 서로 다른 수준으로 사용되며 용도와 묘사 정도에 따라 Sketch 수준, Blueprint 수준, Programming Language 수준으로 나눌 수 있다. 이것은 마치 Use Case가 Brief Format, Casual Format, Fully-Dressed Format으로 나뉘는 것과 유사하다고 할 수 있다.

한 가지 기억해야 할 사실은 UML이 Object-Oriented analysis and design development process가 아니라는 것이다. UML은 단순한 시각화 도구이며 UML을 잘 다룬다고 해서 Object-Oriented way of thinking을 잘 할 수 있는 것은 아니다. 그렇기 때문에 다양한 Object-Oriented 개발 과제를 수행하며 감각을 쌓아 올려야 한다. UML은 생각을 조금 더 잘 표현 할 수 있게 도와주는 것이라는 것을 명심하자.

## Software Development Process and the UP

UP(United Process)는 아래에도 설명하겠지만 OOAD의 De-facto Standard로 불리는 개발 방법론이다. UP는 Inception, Elaboration, Construction, Transition 단계로 나뉜다. Elaboration과 Construction의 한 칸은 각각 3주의 Mini-waterfall 단계를 뜻하며 그 시간동안 Requirement Analysis, Design, Implementation, Test, Release 모든 작업을 주어진 시간 동안 수행하게 된다.

Elaboration phase에서 해결하는 요구사항은 5~10%에 불과하다. 나머지 90~95%에 해당하는 요구사항은 모두 Construction phase에서 해결하게 된다. 그치만 요구사항 자체는 Elaboration phase에서 모두 확정되어야 한다. 이렇게 확정된 요구사항들은 각 Iteration마다 Incremental, Evolutionary 한 개발을 통해 구체화 된다. 아래는 이렇게 설명한 단계들의 특징을 정리한 내용이다.

1. **Inception:** approximate vision, business case, scope, vague cost estimates
2. **Elaboration:** refined vision, iterative implementation of the core architecture, resolution of high risks, identification of most requirements and scope, more realistc estimates
3. **Construction:** iterative implementation of the remaining lower risk and easier elements, preparation for deployment
4. **Transition:** beta tests, deployment

각 단계에서 어떤 수준으로 진행되는지를 개략적으로 표현해주는 모식도를 가지고 왔다. 실제로 아래와 같이 프로젝트 단계별로 인력을 배치해 MM(Man-Month) 효율을 추구하는 것이 가장 이상적이다.

<img src="/_img/2022-08-22/up-disciplines.png">

이를 단계별로 조금 더 세부적으로 표현한 표는 아래와 같다. 각 Phase 칸은 공백과 S와 R이 적혀있는 것을 볼 수 있다. 여기서 S는 Start, R은 Refinement의 약자로 각각 생성과 수정을 뜻한다. 그리고 가장 중요한 공백은 더 이상 수정 불가능함을 뜻한다. 가령 SW Architecture Document는 Elaboration Phase 이후 절대 수정되면 안 되기 때문에 Construction Phase 칸에는 빈칸으로 나타나있다.

<img src="/_img/2022-08-22/table-sample-development-case-s-start-r-refine.png">

### System Context and Interactions

객체지향 소프트웨어 설계를 위한 분석의 첫 단계는 개발 대상인 시스템과 소프트웨어와 주변 환경이 어떻게 상호작용하는가를 파악하는 것이다. 이를 위해 시스템의 경계를 그린 다음 주변 환경과 어떤 기능을 주고 받는지를 개략적으로 도식화는데 이를 System Context Model 이라 한다.

한 단계 더 나아간 Interaction Model은 단순하게 연결된 System Context Model의 관계가 상세히 어떤 동작을 하는지 다이어그램으로 나타낸다. 보통 Use-case model 로 이를 표현할 수 있다.

### Architectural Design

보통 이전 단계까지를 OOA로 보며 이후부터는 분석된 결과를 토대로 시스템을 설계하는 과정을 가진다. 시스템과 주변 환경과의 상호작용을 정의했다면, 설계 첫 번재 과정으로 해당 상호작용을 수행하는 주체와 이에 필요한 요소들을 정의한다.

### Objec Class Identification

OOD 과정에서 가장 어려운 부분이다. 정형화된 공식은 없으며 기술과 경험, 배경지식에 따라 좌우되는 단계이다. Object identification 단계는 Iterative process 로 계속해서 수정될 수 있으며, 대표적으로 Domain Model을 사용한다. ERD의 Entity 정의와 유사하며, 각각이 가진 Attribute 만을 기재한다.

### Design Model

이전까지 어떤 Entity 들이 시스템을 개발하는데 사용되는지 파악했다면, 본격적으로 시스템을 대표하는 모델을 설계하는 단계다. 이 장에서는 Object와 Object class들 사이의 관계에 초점을 둔다. 이 때, 모델 설계는 아래와 같이 두 가지 타입으로 나뉜다.

- Structural (Static) model
    - 정적인 상태에서의 오브젝트들 간의 관계을 모델링
    - Class diagram, Object diagram, Package diagram
- Dynamic model
    - 실행 단계에서 오브젝트들 간의 상호작용을 모델링
    - Sequence diagram, Communication diagram, Statechart diagram

### Interface Specification

끝으로 각 오브젝트들 사이에 어떤 인터페이스가 존재하는지를 명세한다. 최종적으로 Class diagram 산출물을 만드는 작업이 수행된다. 해당 단계는 OOI(Object-Oriented Implementation)에 해당되는 작업으로 같은 다이어그램과 프로젝트 문서를 보고서도 서로 다른 산출물이 작성되기도 한다.

## An OOAD Example - UP(United Process)

앞서 설명한 UP를 통해 OOAD 과정을 전반적으로 살펴보려고 한다. 먼저 **Use case** 기반 **Domain Model** 작성을 수행한 후 **Sequence Diagram** 을 그리며 시스템 동작을 묘사한다. 이 때, 각 Operation을 시간에 따른 동작 순서로 명세해 Role이 가진 Operation을 모두 찾아내고, 이를 토대로 Class Diagram 을 그린다. 

이를 Iterative Model 의 de-facto industry standard 인 UP(United Process) 각 단계에 대응시켜 설명해보자. UP는 먼저 독립적으로 설계, 개발, 시험된 부분을 합쳐 하나의 소프트웨어를 만드는 방식인 Incrementally Iterative 방식과 지속적인 설계, 개발, 시험을 통해 품질을 향상시켜 나가는 Evolutionarily Iterative 방식 모두를 취하고 있다. 중요한 것은 각 Elaboration 단계에서 발생하는 매 Iteration 기간은 무조건 3주에 걸쳐 진행된다는 것이다.

<img src="/_img/2022-08-22/united-process.png">

3주동안 OOAD 모든 과정을 통해 고객에게 전달할 프로토타입을 제작하게 된다. 즉, 요구사항을 취합하고 Domain Model 을 만들고 Use-case 로부터 Class Diagram 과 Interaction Diagram 을 작성한다. 그리고 작성된 산출물을 기준으로 개발을 진행한다. 3주 뒤 만들어진 산출물을 고객에게 전체적으로 컨펌받고 나서야 Elaboration 과정이 종료된다.

물론 무분별하게 특정한 Use-case를 골라 개발하는 것은 아니라는 점을 명심하자. Elaboration 은 Risk Driven 으로 진행된다. 이 때 발생할 수 있는 Risk 는 Client-centric 한 문제와 Architecture-centric 한 문제로 나뉜다. 간단하게 설명하자면 **고객이 가장 중점적으로 생각하는 기능이 구현되도록** 개발하며 **Architectural Requirement 를 모두 만족할 수 있도록** 개발한다.

예를 들어, 100개의 구현 기능이 있는데 90번째까지 Server/Client 구조로 개발했으나, 91번째 기능을 구현하려 해보니 Stand-alone 으로만 구현할 수 있는 상황이 발생할 수 있다. 이렇게 끔찍한 일이 벌어지지 않도록 아키텍처 리스크를 고려해 가장 중요한 것을 먼저 개발하는 것을 Architecture-centric Risk driven 개발이라 한다.

그리고 Construction 영역에서는 Architecture non-sensitive 한 나머지 영역을 개발해 취합하게 된다. 몇 번을 강조해도 부족하지만 Construction phase에서는 요구사항과 Architecture는 더 이상 수정될 수 없다는 것을 원칙으로 삼아야 한다. 그러나 안타깝게도 현업의 요구사항 추가에 따라 이들이 달라지는 경우가 생기기 마련이다.

## Prepare

**사용자의 요구사항은 항상 변화한다. 그게 싫으면 프로그래머라는 직업을 관둬라.** 라는 말을 들었다. 개발자이며 엔지니어인 우리는 이 굴레에서 벗어날 수 없다고 생각한다. 그렇기 때문에 도메인을 이해하고, 이에 따라 나타날 수 있는 변화를 예측해 그들의 요구사항에 대해 민감하고 유연하게 대응할 수 있는 설계 방법을 배우며 발전해나가야 한다. 그 방법은 Design Pattern이 될 수 있고 다른 여러 개발 사상을 접하는 것도 될 수 있다. 중요한 점은 지속적으로 성장해나가는 것이다.

개념을 아는 것도 중요하지만 그만큼 그림을 그려보는 경험 역시 중요하다. 언젠가 프로젝트가 시작되고, 경력이 채워지면 지금까지 배웠던 과정을 직접 수행해보고 싶은 마음이 든다. 얼른 풍부한 개발 경험을 쌓아 Project Managing 영역으로 넘어가보고 싶다.