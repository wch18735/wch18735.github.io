---
title: "[SW Engineering] Agile"
excerpt: "agile software development"
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

# Agile Software Development

먼저 Agile methods 와 Agile devleopment methods 는 다르다. 전자는 애자일 방법론 자체이며, 후자는 애자일 성격을 띄는 개발 방법론이다. 둘은 엄연히 다르다 할 수 있다. 더 쉽게 설명하자면 애자일 방법론은 Agile Development Techniques 과 Agile Project Management 로 나눌 수 있다.

그러나 개발 방법론과 프로젝트 관리 모두 *Agile 스럽게* 진행하는 것을 요즘 대기업들이 원하고 있다. 그렇다면 Agile 스럼다는 무엇일까. 아래 네 가지 Agile Manifesto 를 보면 어렴풋이 감을 잡을 수 있다.

- **Individual and interactions** over process and tools
- **Working software** over comprehensive documentation
- **Customer collaboration** over contract negotiation
- **Responding to change** over following a plan

위 네 가지 정신을 기억하며 Agile Software Devleopment methods 를 살펴보자. Working software (동작하는 소프트웨어)를 전달하는 Rapid development and delivery 는 소프트웨어 시스템 개발에 요구되는 중요한 요구사항이다. Agile development methods(Agile 개발 방법론)은 소프트웨어 시스템 전달을 빠르게 수행하기 위한 프로세스를 가진다. Agile 개발 방법론의 특징은 아래와 같다.

- 소프트웨어 개발은 **이해 관계자의 참여**를 전제로 일련읜 incremental 또는 evolutionary 한 방법으로 진행된다.
- 빈번한 S/W 전달을 가정한다
- 문서 산출물 관리를 최소로 유지한다

## Agile Devlopment Techniques

Agile 개발 방법론 중 대표적으로 불리는 XP(eXtreme Programming)에 대해 알아보자. 익스트림 프로그래밍(eXtreme Programming, XP)는 켄트 백 등이 제안한 소프트웨어 개발 방법이다. 비즈니스 상의 요구가 시시각각 변동이 심한 경우에 적합한 개발 방법이다. 1999년 켄트 백의 저서인 'Extreme Programming Explained - Embrace Change'에서 발표되었다. 애자일 개발 프로세스라 불리는 개발 방법 중의 대표적인 하나로 꼽히며, 약칭인 'XP'로 잘 알려져 있다.

10~12개 정도의 구체적인 실천 방법(Practice)을 정의하고 있어, 비교적 적은 규모의 인원의 개발 프로젝트에 적용하기 좋다. 개발 문서 보다는 소스코드를, 조직적인 개발의 움직임 보다는 개개인의 책임과 용기에 중점을 두는 경향이 크다.

XP 가 가지는 Principles 를 아래와 같이 나열할 수 있다.

- Incremental development is supported through small, frequent system release
- Customer involvement means full-time customer engagement with the team
- Collective ownership thourgh pair programming
- Change supported through regular system release
- Maintaining simplicity through constant refactoring

이러한 원리를 수행하기 위한 실천 사항을 아래와 같이 나타낼 수 있다. 읽다보면 알겠지만 우리가 프로젝트 회의를 진행하며 들었던 많은 단어들을 볼 수 있다. <span style="font-size: 0.8em">(출처: Wikipedia)</span>

<span style="font-size: 1.2em; font-weight: bold;">Whole Team</span>
애자일 방법론과 XP 방법론이 많이 사용되기 이전에는 팀을 기반으로한 소프트웨어 개발이 흔치 않았다. 그렇기 때문에 개발자들과 프로젝트에 참여하는 관리자 모두 이러한 방법론에 대해서 진지하게 생각을 하지 않고 있었다. 그래서 고전적인 소프트웨어 개발 방법론인 폭포수 모델 과 나선 모형에서는 수많은 소프트웨어 개발의 실패를 불러오기도 했다. 하지만 애자일 방법론 및 XP 방법론이 유명세를 타기 시작하면서 팀을 기반으로한 협동적인 개발 방법론이 강조되기 시작했다. 그러므로, 첫 번째 실천 방법인 Whole Team 은 모든 프로젝트에 참여하는 팀원들을 가리키며 개개인이 각자의 역할이 있고 그들의 역할의 중요성을 이야기 한다. XP에서 말하는 Whole Team은 일반적으로 tester, interaction designers, architects, project managers, product managers, executives, technical writers, programmers and users 로 구성이 된다. 이들 중에서 가장 중요한 팀원은 사용자(user)이다. 왜냐하면 그들이 프로젝트의 키를 가지고 있는(stakeholder)일 뿐만 아니라 그들을 통해 요구사항(requirement)을 파악 할 수 있기 때문이다.

<span style="font-size: 1.2em; font-weight: bold;">Planning Game</span>
XP에서 계획을 세우는데에는 중요한 2가지가 있다. 첫 번째, 이번 반복(iteration)에는 어떤 개발 과정을 끝마칠 것인가. 두 번째, 그 이후 개발 반복(iteration)에서는 무엇을 할것인가이다. XP는 일반적으로 2주를 주기로 계획을 세우고 프로토 타입을 만들어서 최종 사용자 혹은 프로젝트를 의뢰한 의뢰인과 함께 무엇이 얼만큼 개발이 되었는지 혹은 알맞은 방향으로 개발이 진행이 되어가고 있는지 검사 혹은 회의를 한다. 그렇기 때문에 그 기한안에 프로젝트 혹은 프로토 타입은 반드시 개발이 완료가 되어야 하며, 그렇지 못해서 기한을 연장하게 되면, 그에따른 추가 비용및 시간적 손실이 발생하게 된다. 또한, 이를 통해서 소프트웨어 개발의 투명성을 확보 할 수가 있다. 기업의 입장에서 좋은 점은 그들의 실력및 가능성을 보여줄수 있는 기회로 사용될 수 있으며, 사용자 혹은 의뢰인 입장에서는 더욱 큰 금전적 손실이 발생하기 전에 프로젝트를 취소하고 다른 개발팀을 찾아 볼 수 있는 기회로 사용될 수도 있다.

<span style="font-size: 1.2em; font-weight: bold;">Customer Tests</span>
가장 흔히 발생하는 소프트웨어 개발 실패중 하나는 개발이 끝난 제품 혹은 소프트웨어가 처음 사용자 혹은 의뢰인이 원했던것과 다르다는 것이다. 그렇기 때문에 XP에서는 반복적으로 커스토머 테스트를 거친다. 그리고 이를 통해서 그들이 잘못 이해하고 있었던 부분에 대해서 수정을 거치기도 하며 더욱더 의뢰인 혹은 최종 사용자의 요구에 부합하는 소프트웨어를 만들어 내게 된다.

<span style="font-size: 1.2em; font-weight: bold;">Small Releases</span>
이 실천 방안을 통해서 개발자는 주기적으로 프로토 타입을 의뢰인에게 보여준다. 그리고 이를 통해서 의뢰인은 제한된 기능을 가지고 있지만 실제로 작동이 되는 데모 모델을 볼 수가 있으며 추가 사항을 요구 할 수도 있다. 기존의 혹은 과거의 소프트웨어 개발 과정에서 개발이 끝나기 이전에 어떤 소프트웨어가 만들어질지 알 수 없었던 것과 비교해서 가장 차이점을 만들어내는 실천 방안이다. 또한 개발자에게 있어서는 현재까지의 개발 상황이 올바른 길로 가고 있음을 알 수 있다.

<span style="font-size: 1.2em; font-weight: bold;">Simple Design</span>
보통의 프로젝트 개발 혹은 코딩의 경우 기능이 더해지면 더 해질 수록 복잡해 지기 마련이다. 그렇게 되면, 새롭게 충원되는 개발자의 경우에 현재까지의 개발 상황을 이해하는데 오랜 시간이 걸리게 된다. 또한 버그가 발생할 경우에도 쉽게 처리를 하지 못하고 오랜 시간이 걸리게 된다. 그렇기 때문에 XP에서 모든 코딩을 가능한 간단하게 할것을 강조한다. 이러한 추세 혹은 트렌드는 KISS 원칙 원리와도 함께 한다고 할 수 있다.

<span style="font-size: 1.2em; font-weight: bold;">Test-Driven Development</span>
테스트를 기반으로한 개발은 XP에서 가장 중요한 실천 방안중 하나이다. 테스트를 거치고 코딩을 하며 프로젝트를 개발해 나간다.

<span style="font-size: 1.2em; font-weight: bold;">Pair Programming</span>
두명 혹은 그 이상의 프로그래머가 함께 코딩을 하는 것을 말한다. 두명의 프로그래머가 함께 코딩을 하고 테스트를 통해서 개발을 할 수도 있고, 한명은 코딩을 하고 한명은 Quality Assurance 역할 통해서 테스트에만 집중을 할 수도 있다.

그러나 현실적으로 XP 모든 실천 사항을 개발에 적용하는 것은 어렵다. 그렇기 때문에 각각의 요소가 다양한 개발 방법 형태로 이용되고 있다.

- User Stories for specification
- Refactoring
- Test-first development
- Pair Programming

## Agile Project Management

앞서 살펴본 Agile 개발 방법론에 이어 프로젝트 관리에 활용되는 Agile 을 알아본다. Agile 개발 방법론의 한 형태로 XP가 있었던 것처럼 Agile 프로젝트 관리 방법의 한 형태로 Scrum 이 있다.

프로젝트 매니저(a.k.a PM)은 소프트웨어를 정해진 납기일 안에 요구사항을 만족하는 형태로 전달해야 할 의무가 있다. 이것이 PM의 가장 큰 의무이자 책임이다. 이전의 Projec Management 는 모두 plan-driven 한 형태를 띄었다. 대표적으로 Waterfall 방식으로 프로젝트를 관리했다.

Agile 프로젝트 관리 방법은 기존 plan-driven 방식과는 다른 방법으로 프로젝트 관리를 수행한다. 그 대표적인 Scrum 은 다른 어떤 Agile 의 핵심 요소들 중에서도 iterative development 를 관리하는 것에 중점을 준다.

Scrum 은 또한 a short daily meeting 을 뜻하는 단어이기도 하다. 모든 팀 멤버가 아침에 모여 정보를 공유, 이슈 사항, 진척도 등을 공유한다. 그리고 당일 어떤 일을 수행할지 계획하는 시간을 갖는다.

이런 Scrum 은 세 가지 단계를 가진다.

- Initial phase
    - 소프트웨어 아키텍처와 디자인 등의 목표를 수립하는 시기
- A series of sprint cycle
    - 2~4주 주기의 스프린트로 release 를 만들어 공유하는 시기
- Project closure phase
    - 문서를 정리하고, 프로젝트로 부터 얻은 경험을 공유하는 시기

Scrum 에서 사용하는 용어는 아래와 같다.

- **제품 백로그(Product Backlog):** 개발할 제품에 대한 요구 사항 목록
- **스프린트(Sprint):** 반복적인 개발 주기 (회사에서 정하는 이터레이션이 개발 주기가 된다. 계획 회의 부터 제품 리뷰가 진행 되는 날짜 까지의 기간이 1스프린트 이다)
- **스프린트 계획 회의(Sprint Planning Meeting):** 스프린트 목표와 스프린트 백로그를 계획하는 회의
- **스프린트 백로그(Sprint Backlog):** 각각의 스프린트 목표에 도달하기 위해 필요한 작업 목록
- ***일일 스크럼 회의(Daily Scrum Meeting):*** 날마다 진행되는 미팅 (어제 한일, 오늘 할일, 장애 현상 등을 공유)
- **실행 가능한 제품(shippable product) 개발:**스프린트의 결과로써 나오는 실행 가능한 제품
- **제품 책임자(Product Owner):** 제품 백 로그를 정의하여 우선순위를 정해 준다.
- **스크럼 마스터(Scrum Master):** 프로젝트 관리자(코치)

