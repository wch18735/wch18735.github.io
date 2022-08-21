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
author: 1FeS
comments: true
---

# Object-Oriented Analysis and Design

소프트웨어 설계를 위해서는 다양한 설계 프로세스가 있지만, 객체 지향 시스템을 설계하기 위한 프로세스는 대부분 OOAD 를 따른다. 물론 OOAD 가 다른 설계 프로세스와 크게 차이나는 것은 아니다. 객체 지향 설계 프로세스는 대부분 아래와 같은 공통 과정을 가진다.

- Define the context and modes of use of the system
- Desing the system architecture
- Identify the principal system object
- Develop design model
- Specify object interface

OOAD는 OOA(Object Oriented Analysis)와 OOD(Objec Oriented Design) 두 단계로 구분된다. 즉, 분석을 수행한 후 분석 결과에 따라 설계를 수행하게 된다. 개발은 분석과 설계가 모두 끝난 후 진행한다는 것을 명심해야 한다. 그렇기 때문에 개발자에서 엔지니어가 되기 위해서는 단순 기술 적용 단계를 넘어 더 높은 수준의 분석, 설계 역량을 갖춰 나가는 것을 목표로 해야한다.

## System Context and Interactions

객체지향 소프트웨어 설계를 위한 분석의 첫 단계는 개발 대상인 시스템, 소프트웨어와 주변 환경이 어떻게 상호작용하는가를 파악하는 것이다. 이를 위해 시스템의 경계를 그린 다음 주변 환경과 어떤 기능을 주고 받는지를 개략적으로 도식화는데, 이를 System Context Model 이라 한다.

한 단계 더 나아간 Interaction Model 은 단순하게 연결된 System Context Model 의 관계가 상세히 어떤 동작을 하는지 다이어그램으로 나타낸다. 보통 Use-case model 로 이를 표현할 수 있다.

## Architectural Design

보통 이전 단계까지를 OOA로 보며 이후부터는 분석된 결과를 토대로 시스템을 설계하는 과정을 가진다. 시스템과 주변 환경과의 상호작용을 정의했다면, 설계 첫 번재 과정으로 해당 상호작용을 수행하는 주체와 이에 필요한 요소들을 정의한다.

## Objec Class Identification

OOD 과정에서 가장 어려운 부분이다. 정형화된 공식은 없으며 기술과 경험, 배경지식에 따라 좌우되는 단계이다. Objec identification 단계는 Iterative process 로 계속해서 수정될 수 있으며, 대표적으로 Domain Model 을 사용한다. ERD 의 Entity 정의와 유사하며, 각각이 가진 Attribute 만을 기재한다.

## Design Model

이전까지 어떤 Entity 들이 시스템을 개발하는데 사용되는지 파악했다면, 본격적으로 시스템을 대표하는 모델을 설계하는 단계다. 이 장에서는 Object와 Object class들 사이의 관계에 초점을 둔다. 이 때, 모델 설계는 아래와 같이 두 가지 타입으로 나뉜다.

- Structural (Static) model
    - 정적인 상태에서의 오브젝트들 간의 관계을 모델링
    - Class diagram, Object diagram, Package diagram
- Dynamic model
    - 실행 단계에서 오브젝트들 간의 상호작용을 모델링
    - Sequence diagram, Communication diagram, Statechart diagram

## Interface Specification

끝으로 각 오브젝트들 사이에 어떤 인터페이스가 존재하는지를 명세한다. 최종적으로 Class diagram 산출물을 만드는 작업이 수행된다.

## An OOAD Example - UP(United Process)

간단하게 예를 들면 OOAD 과정은 다음과 같은 과정을 거친다. 먼저 `Domain Model 만들기`. 이후 `Use Case + System Sequence Diagram` 를 이용해 생길 수 있는 모든 Operation을 전부 조사한다. 그리고 `Sequence Diagram`으로 각 기능을 시간에 따른 동작 순서로 명세하고, 이를 토대로 한 Class Diagram 을 그리는 것으로 마무리한다. 

이를 Iterative Model 의 de-facto industry standard 인 UP(United Process) 각 단계에 대응시켜 설명해보자. UP는 먼저 Incrementally Iterative 할 수도 Evolutionarily Iterative 할 수도 있다. 중요한 것은 각 Elaboration 단계에서 발생하는 매 Iteration 기간은 무조건 3주에 걸쳐 진행된다는 것이다.

<img src="/_img/2022-08-22/united-process.png">

3주동안 OOAD 모든 과정을 통해 고객에게 전달할 프로토타입을 제작하게 된다. 즉, 요구사항을 취합하고 Domain Model 을 만들고 Use-case 로부터 Class Diagram 과 Interaction Diagram 을 작성한다. 그리고 작성된 산출물을 기준으로 개발을 진행한다. 3주 뒤 만들어진 산출물을 고객에게 전체적으로 컨펌받고 나서야 Elaboration 과정이 종료된다.

물론 무분별하게 특정한 Use-case 를 골라 개발하는 것은 아니라는 점을 명심하자. Elaboration 은 Risk Driven 으로 진행된다. 이 때 발생할 수 있는 Risk 는 Client-centric 한 문제와 Architecture-centric 한 문제로 나뉜다. 간단하게 설명하자면 **고객이 가장 중점적으로 생각하는 기능이 구현되도록** 개발하며 **Architectural Requirement 를 모두 만족할 수 있도록** 개발한다.

예를 들어, 100개의 구현 기능이 있는데 90번째까지 Server/Client 구조로 개발했으나, 91번째 기능을 구현하려 해보니 Stand-alone 으로만 구현할 수 있는 상황이 발생할 수 있다. 이렇게 끔찍한 일이 벌어지지 않도록 아키텍처 리스크를 고려해 가장 중요한 것을 먼저 개발하는 것을 Architecture-centric Risk driven 개발이라 한다.

그리고 Construction 영역에서는 Architecture non-sensitive 한 나머지 영역을 개발해 취합하게 된다.

## After Lecture

이후에도 Design Pattern 에 대해 짧게, Implementation Issue 에 관해 짧게 배우는 시간을 가지고 UML 다이어그램에서 다루는 다양한 Diagram 종류에 관한 설명을 들었다. 

개념을 아는 것도 중요하지만 그만큼 그림을 그려보는 경험 역시 중요하다는 말씀이 기억에 남는다. 언젠가 프로젝트가 시작되고, 경력이 채워지면 지금까지 배웠던 과정을 직접 수행해보고 싶은 마음이 든다. 얼른 풍부한 개발 경험을 쌓아 Project Managing 영역으로 넘어가보고 싶다.