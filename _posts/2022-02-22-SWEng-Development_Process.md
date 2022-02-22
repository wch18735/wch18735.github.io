---
title: "[SW Engineering] Software development process"
excerpt: "software development process"
date: 2022-02-22
layout: single
classes: wide
category:
    - software engineering
tag:
    - software engineering
author: 1FeS
comments: true
---

# Software Development Process

Software process 는 보다 나은 소프트웨어를 개발하도록 구조적으로 설계된 **과정**을 뜻한다. 요구사항 명세 확정, 소프트웨어 구조 설계, 품질 검증, 유지 및 보수 등의 과정들 각각이 모두 Software Process 이다. Software process model 은 **set of activites**로 설명할 수 있으며, 이런 과정들이 종합된 추상적인 모델들을 뜻하는 아래와 같은 종류들이 있다. 

- Waterfall
- Incremental
- Evolutionary
- Spiral
- CBD (Component-Based Development)
- Iterative - Agile
- Iterative - RUP (Rational Unified Process)

각각은 SW Development Lif-Cycle 모델로 말할 수 있다. 2000년대부터 현재까지 위의 다양한 모델들이 Waterfall Model 과 Iterative Model 두 가지 형태로 귀결되었다. 현재 한창 유행하고 있는 Agile 방식도 Iterative SDLC 모델의 일종이다.

## The Waterfall Model

1960년대 제안된 고전 모델(Classic software development life-cycle model). 소프트웨어 개발을 체계적이고 연속적인 관점으로 접근한 최초의 시도였다. 구분 가능한 단계들로 분리되는데, 이전 단계가 끝나기 전에는 다음 단계로 나아갈 수 없는 것이 특징이다. 그렇기 때문에 변화에 대한 사용자의 요구사항이 쉽게 반영되지 않는 단점이 있다.

단점이 강하다고 생각되겠지만 대규모 시스템 구축 프로젝트는 모두 Waterfall 모델 기반으로 이뤄진다. Hipass, 전철 시스템, 교통 관제 시스템 등에 사용되는 대규모 소프트웨어는 수십개의 회사가 참여해 각 단계마다 엄청난 회의와 논의를 거쳐 체계적인 구축 과정을 거친다. 생각해보면... 요구사항이 굉장히 명확한 대규모 시스템은 사용자 요구사항이 크게 변화할 일이 없으며, 각 단계에서 발생하는 개발 산출물을 통한 관리, 기록이 아주 중요한 측면이 될 수 있다.

위 모델이 유용한 상황은 다음과 같이 정리할 수 있다.

- 명확한 요구사항이 이전에 정의됐을 때
- 선형적인 방법으로 모든 프로젝트가 진행될 수 있을 때
- 다양한 측면에서 개발이 진행되는 대규모 프로젝트

## The Incremental and Evolutionary Model

Incremental development 모델은 세부적인 요구사항이 모두 정해졌을 때, 평행하게 Release 들을 개발 과정을 거친다. `design → code → test → integrate → O&M` 과정을 거친 Release 들은 모두 마지막에 합쳐지게 된다.

반면 Evolutionary development 에서는 소프트웨어가 Version 별로 관리된다. 이전과는 다르게 마지막에 남는 버전이 최종 결과가 된다. 이전 버전은 관계있는 다음 버전 내부 관련있는 단계로 병합된다.

<img src="/_img/2022-02-22/evolutionary_process_model.png">

둘 중 하나의 형태를 취하는 경우는 거의 없으며, 보통 두 개의 특징을 모두 활용한다. 

## The Spiral Model

이는 Waterfall 모델에서 Risk Analysis 과정이 추가된 형태로 이해할 수 있다. 일종의 evolutionary model 의 프로토타이핑과 체계적인 시스템 중심 진행의 waterfall 형태의 결합이다.

<img src="/_img/2022-02-22/spiral_model.png">

## CBD (Component-Based Development)

이는 software reuse 에 기반을 둔 모델이다. 존재하는 어플리케이션 시스템 컴포넌트를 가져와 활용하기 때문에 장점과 단점이 명확하다고 할 수 있다. 검증된 소프트웨어 컴포넌트의 기능을 그대로 활용할 수 있으며, 시간을 절약할 수 있다는 것이 장점이다. 반면 비주도적 업데이트가 수반될 수 있다는 것과 컴포넌트 적용 초기 원하는 형태로 데이터를 주고받기 위한 테일러링이 필요하다는 것이다.

## Agile

Agile 이라는 단어는 특정 프로세스를 뜻하는 것이 아니라 하나의 통칭이다. 이는 `rapid prototyping` 과 `rapid development` 에 중점을 두는 방법론을 뜻한다. 중점적으로 여기는 가치는 경량화된 문서화 수준과 이해관계자(Stakeholder)와의 협업이다. 이를 조금 더 자세히 나타내보면 아래와 같이 Agile Manifesto 를 나타낼 수 있다. 

- **Individual** over processes and tools
- **Working software** over documentation
- **Customer collaboration** over contract negotiation
- **Responding to change** over following a plan

## RUP (Rational Unified Process)

 Rational Unified Process 또는 Unified Process 라 불리는 모델은 Iterative 한 특징을 가진다. 해당 모델을 이끄는 몇 가지 관점을 정리해보면 아래와 같다. 이는 Object Oriented 소프트웨어 개발의 de-facto industry standard 로 자리잡고 있다. 

 - Risk-driven
 - Client-driven
 - Architecture-centric
 - Use-case-driven

# Process Activities

앞에서 설명한 Process Model 들은 일을 어떻게 하느냐에 대한 순서를 나타낸다. 그러나 `어떤 일을 하느냐` 는 결국 공통적이다. 아래 네 가지가 모든 프롯세스 모델에서 다루는 공통적인 Activity, 활동들이다. Activity 아래 항목들은 각 활동들을 위해 수행하는 세부적인 내용을 담은 키워드들이다.

- Specification
    - Requirements Engineering
- Development
    - Design Activities
    - Implementaion Activities
        - SASD (Structured Analysis and Structured Design) for Procedural Programming
        - OOAD (Object-Oriented Analysis and Design) for Object-Oriented Programming
- Validation
    - V-model Software testing
- Evolution
    - Change software by seperated level of system (data / code / architecture / design)

## Process Improvement

우리가 프로젝트를 진행할 때, 어떤 프로세스 모델을 선택하였는가에 관계없이 위 Activity 들이 제대로 수행되고 있는지를 확인하고 개선하는 작업이 수반되어야 한다. 이 때, 가장 대표적인 지표로 CMMi 레벨이 있다. SEI(Software Engineering Institute) 의 CMMi (Capability Maturity Model Integraged) 레벨은 조직의 프로세스 관리가 얼마나 성숙히 이뤄지고 있는지를 뜻한다. 레벨 하나 올리는데 약 10년 ~ 15년 정도 시간이 걸린다고 한다...