---
title: "[SW Engineering] UP - Inception Phase"
excerpt: "united process inception phase"
date: 2022-09-15
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

# Inception

Inception을 간단하게 설명하자면 개발하려는 소프트웨어 또는 시스템의 대략적인 내용을 요약해 기획팀에게 보내기 위해 수행하는 한 주 짜리 짧은 스터디라고 생각 할 수 있다. 이런 Inception 단계에서는 과제 참여자들이 아래와 같은 질문들을 간단하게 생각해보는 시간을 가진다. 여기서 중요한 것은 대다수의 사용자 요구 사항은 Inception Phase가 아닌 Elaboration Phase에서 분석된다는 것이다.

- What is the vision and business case for this project?
- Feasible?
- Buy and/or Build?
- Rough unreliable range of cost
- Should we proceed this project or stop?

이러한 질문에 답하며, Inception Phase에서는 Funcional Requirements와 Non-funcitonal Requirements를 수집하는데, 각각은 주로 Use-Case Model과 Supplementary Specification이라는 형태로 간략히 작성된다. 여기에 더해 아래와 같은 다양한 Artifact들이 작성될 수 있는데, 이들은 UML 모델링을 요구하지 않으며, 필요하다고 해도 아주 일부 영역에서만 활용되기 때문에 UML 모델링 언어가 활용되는 부분은 미미하다고 할 수 있다.

<img src="/_img/2022-09-15/table-sample-inception-artifacts.png">

## Evolutionary Requirements

Requirements란 시스템이 반드시 만족해야하는 역량이나 조건을 뜻한다. 보통 Requirements는 명확하다고 생각할 수 있지만, 실제 사용자에게 요구사항을 물어보는 인터뷰를 진행해도 정확한 명세를 얻기는 쉽지 않다. **그들도 그들이 무엇을 원하는지 모른다** 라는 격언이 있듯이 Requirement를 찾아내 정리하는 것은 생각보다 어려운 작업이다.

UP에서는 이러한 요구사항을 **iteratively and skillfuly** 하게 분석한다고 자부한다. 실제로 거의 대부분의 과제들은 Waterfall 방식을 따라가기 때문에 초기 고정된 요구사항의 변화에 취약하다. 반면 UP는 아래와 같은 방식으로 Requirement를 찾아내고, 실제 사용자에게 모든 요구사항이 만족될 때까지 Iteration을 반복한다. 그리고 그렇게 분석된 요구사항들은 FURPS+ model에 따라 분류해 Checklist에 따라 검수를 수행한다.

- writing use cases <u>with customers</u>
- <u>requirements workshops</u> that include both developers and customers
- <u>a domo of the results</u> of each iteration to the customers, to solicit feedback

특히 Non-functional Requirements 중 개발하려는 시스템 또는 Stakeholder가 중요하게 생각하는 속성인 Quality Attributes/Requirements를 어떤 수준으로 만족시키느냐에 따라 소프트웨어 Architecture가 바뀌는 경우도 있다. 예를 들어 특정 회사는 해당 회사에서 제공하는 계열사의 클라우드만은 사용해야 한다는 제약이 걸려있을 수 있다. 이런 조건들 하나하나가 모두 NFR이 될 수 있다. 그렇기 때문에 기능적 측면보다 품질적 측면이 개발에 더 큰 영향을 미친다 얘기가 틀린 것만은 아니라 할 수 있다.

## Use Cases

도메인에 대한 이해와 시스템을 설계하기 위해 Use Case를 작성하게 되는데 [Use Case Diagram - 1FeS Study Note](https://wch18735.github.io/software%20engineering/SWEng_Use_Case_Diagram/)에 잘 정리해 놓았다. 참고로 Inception에서는 Brief Format까지만 작성되며 Elaboration Phase나 Construction Phase에서 Fully Dressed Format까지 진행한다.