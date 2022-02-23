---
title: "[SW Engineering] Requirements Engineering"
excerpt: "requirements engineering"
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

# Requirements Engineering

요구 공학으로 불리는 Requirements Engineering 은 Specification 과정에 활용된다. Customer 가 원하는 요구 사항을 만족시키기 위해 어떤 서비스를 제공할지를 결정해야 한다. 이 단계에서 도출되는 요구사항들을 기반으로 Structure, Architecture 등 많은 것들이 결정된다.

요구 사항은 크게 아래와 같이 2가지로 나눌 수 있다고 한다.

- Functional requirements (FR)
- Non-functional requirements (NFR)

그러나 요구사항 조사 과정 처음부터 모든 요구사항들을 위 형태로 나눌 수는 없다. 그럴리가 없다. 초기 사용자들로부터 받아오는 요구사항의 예시는 보통 다음과 같다.

*"음... 대쉬보드가 있으면 좋겠어요."*

*"접속 속도가 좀 빨랐으면 좋겠어요."*

*"깔끔했으면 좋겠어요."*

이렇게 애매모호한 요구사항이 조사되는 것을 Requirements Imprecision 이라 한다. 위 내용을 기반으로 개발을 진행할 수 있을까. 불가능하다. 그렇기 때문에 애매모호한 요구사항들로부터 잠재된 요구사항을 도출할 수 있어야 한다.

그리고 끝에는 각 요구 사항들이 C&C (Completeness and Consistency) 를 가질 수 있도록 정제해야 한다. 이것이 요구 공학의 목적이라 할 수 있다.

<span style="font-size:1.2em">FR (Funcional Requirements)</span>

FR 은 명확하게 기능이 명세된 요구사항들을 말한다. 기능 요구사항이라고 물리는 FR은 반드시 완성되어 V-model 과 같은 테스트 프로세스에서 정량적인 수치로 계산될 수 있는 것들이다.

가령 시스템이 특정한 입력에 대해 어떻게 반응해야 한다라거나 시스템이 특정 상황에 어떻게 동작해야한다 등을 정의한다. FR은 각각에 대한 테스트를 수행할 수 있다.

<span style="font-size:1.2em">NFR (Non-funcional Requirements)</span>

그러나 NFR은 다르다. NFR은 일종의 제약 사항(Constraint)다. 개발을 진행할 때, 외부 개발자들은 특정 계정을 발급받아 진행한다 등의 제한이 예시가 될 수 있다. 여기에 더해 한 가지 Domain Requirements 라는 것이 있다. 특정 도메인에 종속된 NFR의 일종으로 생각할 수 있다.

때로는 NFR 내부에 수치적으로 표현할 수 없는 것들이 위치하기도 한다. Availity, Configurability, Modifiability, Performance, Reliability, Reusability, Security, etc.. 그러나 이들은 완성된 소프트웨어의 품질에 영향을 주는 요소이다. 이런 요소들을 Quality Attributes 라 한다. IEC/ISO 25010 standard 에는 이러한 Qualiy Attributes 들이 기재되어있다.

## Requirements Engineering Process

먼저 RE process 는 소프트웨어가 활용되는 도메인, 개발 프로세스 등에 따라 광범위하게 달라질 수 있다. 그러나 공통적인 네 가지 과정을 아래와 같이 도출해낼 수 있다.

- Requirements elicitation (Requirements discovery)
- Requirements analysis
- Requirements validation
- Requirements change management

## SRS (Software Requirements Specification)

위 과정을 거쳐 나오는 것이 소프트웨어 개발 명세서라 불리는 SRS 이다. 일반적 요구사항에서 세부 FR, NFR을 뽑아내고 최종적으로 User Requirement 를 System Requirement 로 바꾸어 SRS에 작성하는 과정을 System Modeling 이라 한다.

SRS에 꼭 고려되어 추가되어야 하는 요소들은 아래와 같다. 이들이 추가된 SRS Template 이 있는데, IEEE Std 830.1998 에서 확인할 수 있다. 보통의 경우는 아니나 일부 프로젝트에서는 SRS가 Contract(계약서) 역할을 하는 경우도 있다고 한다.

- Funcionality
    - What is the software supposed to do
- External Interface
    - How does tthe software interact with people, the system's hardware, other hardware, and other software?
    - What assumptions can be made about these external entities?
- Rquired Performance
    - What is the speed, availability, response time, recovery time of various software functions, and so on?
- Quality Attributes
    - What are the portability, correctness, maintainability, security, and other considerations?
- Design constraints imposed on an implementation
    - Are there any required standards in effect, implementation language, policies for database integrity, resource limit, operating environment?