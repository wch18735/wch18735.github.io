---
title: "[Java] Reflection API Example"
excerpt: "reflection api"
date: 2023-01-01
layout: single
classes: wide
category:
    - java
tag:
    - java
    - abstract
    - interface
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Reflection

Reflection이란 힙 영역에 로드되어 있는 클래스 타입의 객체를 통해 필드, 메소드, 생성자를 접근 제어자와 상관 없이 사용할 수 있도록 지원하는 API이다. 컴파일 시점이 아닌 런타임 시점에 동적으로 특정 클래스 정보를 추출해낼 수 있는 프로그래밍 기법으로, 프레임워크나 라이브러리 개발 시 주로 사용된다.

Reflection을 사용하는 프레임워크/라이브러리로는 Spring Framework의 DI, Test Framework JUnit, JSON Serialization/Deserialization 라이브러리인 Jackson 등이 있다.

아래 실습은 @Controller 어노테이션이 설정된 모든 클래스를 찾아 출력하는 예제를 다룬다.

## Heap 영역에서 클래스 로드하기

Reflection API 예제를 사용하기 전, 힙 영역에서 클래스를 로드하는 방법을 먼저 알아본다. 힙 영역에서 클래스를 로드하는 방법은 **.class**를 사용하는 방법, **getClass()** 를 사용하는 방법, **Class.forName()** 을 사용하는 방법이 있다. 아래 테스트를 수행하면 반환된 클래스가 모두 같은 것을 알 수 있다.

<img src="/_img/2023-01-01/loadClass test.png">

로드한 클래스에 대해서는 Fields, Constructors, Methods 등을 조사할 수 있다. 아래 예시는 첫 번째 방법으로 User 클래스를 로드한 뒤, 각 요소들을 조사하는 것을 보여준다.

<img src="/_img/2023-01-01/class test.png">

## 어노테이션 만들기

@Controller 어노테이션이 설정된 모든 클래스를 찾는 것이 실습 목표이기 때문에, @Controller 어노테이션을 만들어준다. **@interface** 선언을 통해 작성한 코드가 어노테이션임을 알리고, 해당 어노테이션이 붙을 Target을 ElementType을 통해 전달해준다. 

<img src="/_img/2023-01-01/custom annotation.png">

## Controller Scan

스캔을 수행할 경로를 Reflections()에 전달해 Reflections 객체를 만든다. 그리고 **getTypesAnnotationWith** 메소드를 통해 위에서 작성한 @Controller 어노테이션이 붙은 클래스들을 Set에 모아 놓는다. 이 과정을 수행하는 테스트는 아래와 같이 작성할 수 있다.

<img src="/_img/2023-01-01/controllerScan.png">