---
title: "[Design Pattern] Template Method Pattern (템플릿 메소드 패턴)"
excerpt: "template method pattern"
date: 2022-09-21
layout: single
classes: wide
category:
    - design pattern
tag:
    - software engineering
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
header:
    teaser: "/_img/thumb-nail/software-engineering.png"
---

# Template Method Pattern

Class scope 패턴으로 분류되는 템플릿 패턴의 정의는 아래와 같으며 목적과 사용되는 조건을 아래에 정리하였다.

> The template pattern defines the steps of an algorithm and allows the subclasses to implement one or more of the steps

- Purpose
    - Identifies the framework of an algorithm, allowing implementing classes to define the actual behavior
- Use When
    - A single abstract implementation of an algorithm is needed
    - Common behavior among subclasses should be localized to a common class
    - Parent classes should be able to uniformly invoke behavior in their subclasses
    - Most or all subclasses need to implement the behavior

## Class Coffee and Tea

아래는 커피와 차를 만드는 클래스이다. `void prepareRecipe()`를 살펴보면 중복되는 공통 부분을 확인할 수 있다. 새로운 음료를 추가 할 수록 중복 작성되는 부분이 많아지게 될 것이다. 

<img src="/_img/2022-09-21/coffee and tea.png">

공통적인 부분을 공통 클래스로 묶을 수 있다. 먼저 물을 끓이고, 컵에 붓는 동작은 공통적이기 때문에 이를 추상 클래스에서 구현해 놓고 나머지는 Override를 통해 구현하도록 가이드 할 수 있다. 이 때, 동작들의 세부 사항은 달라지지만 `prepareRecipe()` 순서는 달라지지 않기 때문에 동작 순서를 지정하고 하위 클래스에서 수정을 막기 위해 final 키워드를 앞에 삽입할 수 있다.

<img src="/_img/2022-09-21/CaffeineBeverage.png">

기존 두 개의 서로 다른 클래스와 달리 CaffeineBeverage라는 공통 속성을 가지는 상위 클래스를 두는 구조로 변경되며 바뀐 점과 장점을 아래 정리했다.

- Changes
    - Both subclasses inherit a general algorithm
    - Some methods that perform the same actions for all subclasses in the algorithm are concrete
    - Other methods that perform class-specific actions in the algorithm are abstract

- Advantages
    - A single class protects and controls the algorithm, namely CaffeineBeverage
    - The superclass facilitates reuse of methods
    - Code changes will occur in only one place
    - Other beverage can be easily added

## Template Method

- **prepareRecipe()** implements the template method pattern
    - serves as a template for an algorithm, namely that for making a caffeinated beverage
    - In the template, each step is represented by a method
    - Some methods are implemented in the superclass
    - Other method must be implemented by the subclass and are declared abstract

<img src="/_img/2022-09-21/template method.png">

## Template Method Pattern Structure

<img src="/_img/2022-09-21/template method pattern structure.png">

- Encapsulates an algorithm by creating a template for it
- Defines the skeleton of an algorithm in an operation as a set of steps
- Some methods of the algorithm have to be implemented by the subclasses - these are abstract methods in the super class
- The subclasses can redefine certain steps of the algorithm without changing the algorithm's structure
- Some steps of the algorithm are concrete methods defined in the super class

> ※ Hook Method
A hook is a method that is declared in the abstract class, but only given an empty or default implementation

## Design Principle: Hollywood Principle

템플릿 메소드 패턴과 관련있는 설계 원칙은 Hollywood Principle이다. 이는 상위 컴포넌트가 하위 컴포넌트를 불러 사용하는 것을 말한다. 이를 템플릿 메소드 패턴에 적용해보면 템플릿은 상위 컴포넌트가 되며 나머지 메소드들은 템플릿 속에서 순서가 정해지고 Hook과 함께 어떻게 사용될 것인지 등이 결정된다.

- The Hollywood Principle: Don't call us, we'll call you
    - It prevents "Dependency rot"
    - Dependency rot: high-level components depend on low-level components, and vice versa
- With the Hollywood principle
    - We allow low level components to hook themselves into a system
    - But high level components determine or arrange when they are needed and how
    - High level components give the low level components a "don't call us, we'll call you" treatment

관련된 패턴들은 아래와 같다. 알고리즘 전체가 수정되는 경우는 전략 패턴을 사용하는 것이 좋으며, 알고리즘의 골격을 짜놓고 세부 동작만 변경하는 것이라면 템플릿 메소드 패턴이 권장된다. 그리고 생성 패턴의 Facotry Method Pattern은 이러한 Template Method Pattern의 특화 형태라고 할 수 있다.

- Template Method uses inheritance to vary part of an algorithm
- Strategy uses delegation to vary the entire algorithm.
- Factory Method is a specialization of Template Method