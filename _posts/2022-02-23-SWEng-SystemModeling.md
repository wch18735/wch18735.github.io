---
title: "[SW Engineering] System Modeling"
excerpt: "system modeling"
date: 2022-02-23
layout: single
classes: wide
category:
    - software engineering
tag:
    - software engineering
author: 1FeS
comments: true
---

# System Modeling

시스템 모델링은 시스템을 바라보는 다양한 관점으로 추상화된 모델을 정립하는 과정이다. 시스템 모델링을 통해 소프트웨어에 필요한 System Level Requirements 를 이해하고 구체화 할 수 있다. 또한 모델링된 결과를 바탕으로 사용자와 대화할 수 있다. 대부분의 개념은 UML (Unified Modeling Language) 에 기반하고 있다.

이렇게 설명하면 조금 어렵다. 하나하나 짚어가보자. 먼저 다양한 관점. 시스템을 바라보는 관점은 아래와 같다.

- External perspective: 시스템 문맥을 모델링 하는 관점
- Interaction perspective: 시스템과 주변 컴포넌트, 환경의 상호작용을 모델링하는 관점
- Structural perspective: 시스템의 데이터 처리 절차를 모델링하는 관점
- Behavioral perspective: 이벤트에 대한 시스템의 동적 행위, 반응을 모델링하는 관점

참고로 위와 같은 관점으로 시스템을 모델링 할 때 사용되는 시각화 도구로 UML diagram 이 있다. 

## External Models

- Context Models
    - 시스템의 기능적 경계를 그림으로 나타냄
    - 시스템 경계에 어떤 외부 요인들이 있는지 기재되는 형태
- Process Models
    - 시스템이 비즈니스 프로세스에 어떻게 사용되는지 나타냄
    - UML activitty diagram 이 시각화 도구로 사용됨

## Interaction Models

- Interaction Models (상호작용 모델링)
    - 사용자 상호작용
    - 시스템 to 시스템 상호작용
    - 컴포넌트들간의 상호작용
    - UML Use-Case diagrm 과 UML Sequence diagram 두 가지가 사용된다

여기서 Use-Case 란 한 개의 시스템 사용자가 시스템을 사용할 때, 어떻게 동작하는지를 나타낸 도표라 할 수 있다. Sequence diagram 은 해당 Use-Case 가 진행되는 절차를 나타낸 시나리오이다.

## Structural Models

- Structral Models
    - 시스템을 구성하는 요소와 요소들간의 관계를 종합한 모델
    - Static Models : 시스템의 정적 조직도
        - Class diagram
    - Dynamic Models : 시스템이 동작할 때를 다루는 동적 조직도
        - Object diagram, Component diagram, Composite structure diagram

일반적으로 Structurarl Models 은 시스템 구조(System Architecture)를 설계할 때 사용된다.

## Behavioral Models

- Behavioral models
    - 시스템이 동작할 때의 행위들을 다룬 모델
    - 외부 자극이나 이벤트에 대한 시스템의 반응을 모델링하는 데 초점을 둔다
- Behavioral Model 종류
    - Data-driven model (Data flow diagram, DFD)
    - Event-driven model (State Finite Machine, SFM)

## Model-Driven Engineering & Model-Driven Architecture

Low-code solution 중 하나인 Mendix를 공부하고, 이를 통해 시스템을 개발하면서 느낀점으로 Low-code solution 은 Modeli-Driven Engineering 영역 중 Development 를 돕는 도구라는 느낌이 강하게 들었다.

Data Model은 ERD, Microflow는 Data Flow Diagram 개념을 그대로 가져다 놓았다. 이렇게 동작이나 데이테베이스 구조를 모식화 해놓은 **모델링**된 모델이 그대로 코드로, 어플리케이션으로 변환되는 형태인 것이다.

해당 장에서 MDE의 장점과 단점이 명확하게 설명되어있다. 장점으로는 시스템의 높은 레벨의 추상화와 자동화된 코드 생성, 단점으로는 추상화된 모델의 구현 적합성 판단 가능성과 타 플랫폼으로의 변환 과정에서의 오버헤드가 있다. 이미 이렇게 정확한 분석이 3년전 문서에 기재됐다는 사실이 놀랍고, 한편으로 정말 소름돋았다.

OJT 주제로 이걸 써볼까... 싶은 생각이 잠깐 스쳤다.