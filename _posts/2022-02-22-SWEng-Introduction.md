---
title: "[SW Engineering] Introduction"
excerpt: "software engineering introduction"
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

# An Introduction to Software Engineering

Software Engineering (소프트웨어 공학)은 **professional** 하고 **cost-effective** 한 소프트웨어 개발을 위한 이론, 방법, 도구 등에 관한 지식을 총 망라하는 학문이라 할 수 있다. 더욱 많은 시스템들이 소프트웨어에 의해 제어되고, 소프트웨어에 대한 의존도가 높아지고 있다. 이에 따라 첨예해지는 기술 스택의 안정적인 적용과 유지 및 보수를 위해서 소프트웨어 공학은 필수적이라 할 수 있다.

## Types of Software Products

Software Products 종류는 두 가지로 정리할 수 있다. 사용자가 구매해 마음대로 사용할 수 있는 Generic software 와 제한된 요구 사항을 만족하는 Customized software, 두 가지다. 둘을 나누는 기준은 Software Requirement Specification(SRS) 가 developer 에게 있는지, customer 에게 있는가이다. 쉽게 말하면 소프트웨어 수정 주체가 누구인가다. 보통의 사내 시스템은 사내 특정 사용자들의 VOC를 받아 수정되기 때문에 사용 주체가 사용자에게 있는 Customized software 라 할 수 있겠다.

## Essential Atributes of Good Software Products

두 종류의 소프트웨어 어느 것이라도 좋은 소프트웨어로 거듭나기 위해서는 아래 조건을 만족해야 한다.

- Maintainability: 좋은 소프트웨어는 소비자의 변화되는 요구 사항을 반영할 수 있도록 작성되어야 한다
- Depnedability: 좋은 소프트웨어는 reliability, security, safety 와 같이 사회에 물리적/경제적 손해를 입히지 않을 수 있어야 한다
- Efficiency: 좋은 소프트웨어는 메모리, 프로세서 등과 같은 자원을 효율적으로 사용해야 한다
- Acceptability: 좋은 소프트웨어는 사용자에게 충분히 수용적일 수 있도록 친화적으로 제작되어야 한다

그러나 모든 조건을 완벽하게 만족하는 소프트웨어를 제작하는 것은 어렵다. 공학 과목을 들을 때면 귀에 딱지가 앉도록 들었던... Trade-off 덕분이다. 만약 Security 를 높이기 위해 안면인식, 터치패드, 방화벽 등의 보안 요소를 강화하면 각각에 대한 Reliability 보장성이 낮아지게 되며, Resource 의 과다한 활용이 야기될 수 있으며 이를 사용하는 사용자에게 친화적이지 못한 경험을 선사할 수 있다. 따라서 그 균형을 잘 조절하는 것이 중요하다.

## Typical Activities in Software Engineering

소프트웨어 공학에서 효율적인 작업을 위해 연구하는 대상을 일반적으로 아래와 같이 정리할 수 있다.

- Software Specification
    - Require Engineering
- Software Development
    - Architecture Design, Detailed Design, Implementation
- Software Validation
    - Software V&V (Verification & Validation), Testing
- Software Evolution
    - Software Maintenance

요구 사항으로부터 필요한 기능들을 정의하고, 이를 개발한 뒤 소프트웨어에 대한 품질 평가를 진행, 끝으로 변화되는 사용자의 요구를 수용해 소프트웨어를 계속해서 변화시키는 모든 단계에 소프트웨어 공학을 적용해 더 좋은 결과를 얻어낼 수 있다.

## Software Project Failure

이런 소프트웨어 공학의 필요성은 **Software projects failure (The software crisis)** 로부터 제창되었다고 한다. 시스템의 규모와 하드웨어 성능 향상으로 시스템 규모와 복잡도가 향상하던 시절, 1960년대 제작된 많은 소프트웨어들이 Fail 하는 경우가 많았다. 그리고 1968년 NATO conference 에서 처음으로 이러한 Software Crisis 를 해결하고자 Software Engineering 이라는 단어와 함께 개발 프로세스가 정립된 모델을 발표한다. Waterfall Model 이다.

이후 다양한 모델들이 나타나고, 각 소프트웨어 특징과 도메인에 따라 변화된 형태로 적용되는 Software Engineering Method 들이 적용된다. 각각은 조금씩 달라도 소프트웨어 공학의 Fundamental 을 포함했다. 모든 타입의 Software Type 들에 공통적으로 적용할 수 있는 기본 원리는 다음과 같다.

- *"Systems should be developed using a managed and understood developed process. Of course, different process are used for different types of software"*
    - SDLC (Software Development Life-cycle)
- *"Dependability and performance are important for all types of system"*
    - Software Quality
- *"Understanding and managing the software specification and requirement are important"*
    - Requirement Engineering
- *"Where appropriate, you should reuse software that has already been developed rather than write new software"*
    - Software Reuse, Open-Source Software

## Recommended References

- The Mythical Man-Month, Frederucj P. Brooks JR
- Software Engineering, Ian Sommervile