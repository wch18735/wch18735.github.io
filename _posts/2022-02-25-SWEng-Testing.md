---
title: "[SW Engineering] Software Testing"
excerpt: "software testing"
date: 2022-02-25
layout: single
classes: wide
category:
    - software engineering
tag:
    - software engineering
    - testing
author: 1FeS
comments: true
---

# Testing

소프트웨어 테스팅 스테이지는 간단하게 세 단계로 나눌 수 있다. 개발하면서 수행하는 Development testing, 사용자들에게 전달하기 전 최종적으로 수행하는 통합테스트 Release testing, 예비 사용자들이 실제 사용환경에서 테스트하는 User testing 이 있다.

이런 테스트 목적은 크게 두 가지로 나눌 수 있다. 하나는 이 프로그램이 의도하는 바가 무엇인지를 아는 것과 이 프로그램의 결함을 찾는 것이다. 이런 테스트를 진행할 때에는 보통 Artificial Data (인공적으로 생성한 데이터)를 사용한다. 한 가지 유의할 점은 해당 테스트 케이스가 잘 못된 부분은 찾을 수 있지만, 검사 범위 이외의 부분에서 에러가 없다는 것을 보장하지는 않는다는 것이다.

그렇기 때문에 테스트는 최대한 세부적으로 이뤄져야하며, `V&V (Verification & Validation)` 프로세스와 액티비티 안의 한 과정으로 체계화 되는 영역 중 하나이다.

## Two Type of Program Testing

프로그램 테스트는 앞서 말한 것 처럼 두 가지 성격을 띈다. 하나는 Validation testing 으로 소프트웨어가 정의된 요구사항을 모두 만족하는가를 시험한다. 다른 하나인 Verification testing 은 개발된 소프트웨어에 결함이 있는가를 테스트 한다.

이를 잘 표현한 질문이 있는데, 먼저 Validation 은 `Does the software system meets the user's real needs?` 이다. 요구 사항을 만족하는가를 묻는 것. Verification 은 `Does the software systems meets the requirements specifications?` 이다. 명세된 기능이 제대로 동작하는지, 결함은 없는지를 검사하는 영역이다.

하나는 Validation 이 안전성, 보안성, 신속성 등의 QA 를 평가한다면 Verification 은 기능, 동작 순서 등을 시험한다. 개발자들은 주로 Verification 에 중점을 두고, Validation 은 QA 부서가 따로 관리를 하는 느낌..?

## 3 Axes of V&V

이런 V&V 과정에서 수행하는 프로세스로 `3 + 1` 을 얘기했다. V&V 의 세 가지 주요 성질인 Optimistic Inaccuracy, Pessimistic Inaccuracy, Simplified Properties 와 매칭되는 세 가지는 Testing, Static Analysis, Model Check 이며 나머지 하나는 Review 다.

결과적으로 위에서 언급한 `3+1` 의 프로세스 각각을 V-model 의 단계에 적절히 적용해 테스트를 수행하고, 검증하여 소프트웨어 품질을 높이는 작업이 최종 목적이라 할 수 있다.

## Devleopment Testing

개발단계에서 수행하는 테스트는 테스트 대상에 따라 Unit, Integrated, System 으로 아래와 같이 나눌 수 있다. 끝으로 Regression 테스트는 코드나 컴포넌트에 변화가 있을 때, 이전 소프트웨어에 영향을 주지 않는 것을 확인하는 과정이다.

- Unit testing
- Integrated testing
- System testing
- Regression testing

## TDD (Test-Driven Devleopment)

Test-driven development 는 기존의 개발 프로세스와는 조금 다른 접근 방법을 보여준다. 코드를 작성하기 전 테스드들을 만들고, 이후 해당 테스트들을 `패스`하는 코드를 작성하는 것이 TDD의 핵심이다. 해당 테스트를 패스하지 못하면 다음 단계로 넘어가지 못하는 것이 특징이다. 자연스레 Unit testing coverage 가 100% 를 맞추게 되어있다.

TDD의 가장 큰 장점으로 소개되는 것은 바로 `Modification Confidence`가 아닐까 싶다. 일단 고쳐봐도 된다는 것. 이상이 생기면 분명 테스트가 패스를 못 할테니까.. 그러나 사실 이 방법은 너무 파격적이라 정말 실력있고 능숙한 개발자들이 모인 집단이 아니라면 정해진 납기까지 소프트웨어를 전달하지 못 할 확률이 높다고 한다. 