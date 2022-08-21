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

# Object-Oriented Design Using UML

객체 지향 시스템을 설계하기 위한 프로세스는 SASD 와는 조금 다르다. 다양한 설계 프로세스가 있지만 객체 지향 설계 프로세스는 아래와 같은 공통 과정을 가진다고 한다.

- Define the context and modes of use of the system
- Desing the system architecture
- Identify the principal system object
- Develop design model
- Specify object interface

이들은 두 단계로 구분되는데 OOA(Object Oriented Analysis)와 OOD(Objec Oriented Design)이다. 즉, 분석을 수행한 후 분석 결과에 따라 설계를 수행하는 것. 명심하자. 개발은 분석과 설계가 모두 끝난 후 진행한다는 것을! 개발자는 단순 기술 적용 단계를 넘어 더 높은 수준의 분석, 설계 역량을 갖춰 나가는 것을 목표로 해야한다는 것을 이번 교육을 통해 철저히 느끼고 있다. 소공, 컴공 친구들은 이 좋은 걸 학부 때 미리 배운다는거지...

## System Context and Interactions

분석을 위해 첫 번째로 수행하는 과정은 개발 대상인 시스템, 소프트웨어와 주변 환경이 어떻게 상호작용하는가를 파악하는 것이다. 이를 위해 시스템의 경계를 그리고 주변 환경과 어떤 기능을 주고 받는지를 개략적으로 도식화 한다. 이를 System Context Model 이라 한다. Interaction Model 은 단순하게 연결된 System Context Model 의 관계가 상세히 어떤 동작을 하는지 다이어그램으로 나타낸다. 보통 Use-case model로 이를 표현할 수 있다.

## Architectural Design

보통 이전 단계까지를 OOA로 보며 이후부터는 분석된 결과를 토대로 시스템을 설계하는 과정을 갖는다. 시스템과 주변 환경과의 상호작용을 정의했다면, 설계 첫 번재 과정으로 해당 상호작용을 수행하는 주체와 이에 필요한 요소들을 정의한다.

## Objec Class Identification

OOD 과정에서 가장 어려운 부분이다. 정형화된 공식은 없으며 기술과 경험, 배경지식에 따라 좌우되는 단계이다. Objec identification 단계는 Iterative process 로 계속해서 수정될 수 있으며, 대표적으로 Domain Model 을 사용한다. Mendix 에서 볼 수 있는 그런 Domain Model 을 말하는 것 같다. 잘 모르는 사람은 ERD의 Entity를 정의한다고 이해하자. 이 때, 각각이 가진 Attribute와 더불어 Method를 함께 기재한다.

## Design Model

위와 같이 어떤 Entity들이 시스템을 개발하는데 사용하는지 파악했다면, 본격적으로 시스템을 대표하는 모델을 설계하는 단계다. 이 장에서는 Object와 Object class들 사이의 관계에 초점을 둔다. 이 때, 모델 설계는 아래와 같이 두 가지 타입으로 나뉜다.

- Structural (Static) model
    - 정적인 상태에서의 오브젝트들 간의 관계을 모델링
    - Class diagram, Object diagram, Package diagram
- Dynamic model
    - 실행 단계에서 오브젝트들 간의 상호작용을 모델링
    - Sequence diagram, Communication diagram, Statechart diagram

## Interface Specification

끝으로 각 오브젝트들 사이에 어떤 인터페이스가 존재하는지를 명세한다. 최종적으로 Class diagram 산출물을 만드는 작업이 수행된다.

## An OOAD Example

간단하게 예를 들면 OOAD 과정은 다음과 같은 과정을 거친다. 먼저 `Domain Model 만들기`. 이후 `Use Case + System Sequence Diagram` 를 이용해 생길 수 있는 모든 Operation을 전부 조사한다. 그리고 `Sequence Diagram`으로 각 기능을 시간에 따른 동작 순서로 명세하고, 이를 토대로 한 Class Diagram 을 그리는 것으로 마무리한다. 

## ETC..

이후에도 Design Pattern 에 대해 짧게, Implementation Issue 에 관해 짧게 배우는 시간을 가지고 UML 다이어그램에서 다루는 다양한 Diagram 종류에 관한 설명을 들었다. 개념을 아는 것도 중요하지만 그만큼 그림을 그려보는 경험 역시 중요하다는 말씀이 기억에 남는다. 언젠가 프로젝트가 시작되고, 짬이 좀 차면... 지금까지 배웠던 과정을 직접 수행해보고 싶은 마음이 든다. Project Managing도 개발만큼 재미있을 것 같은 느낌..? 아니 오히려 개발 경험이 없으면 못 하려나.