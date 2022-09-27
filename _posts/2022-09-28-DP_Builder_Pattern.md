---
title: "[Design Pattern] Builder Pattern (빌더 패턴)"
excerpt: "builder pattern"
date: 2022-09-28
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

# Builder Pattern

Builder Pattern 중 Director와 Builder의 관계는 전략 패턴은 생성에 적용한 패턴이라고도 할 수 있다. 즉, 다양한 생성 알고리즘을 구현하는 ConcreteBuilder는 그 구현체 수 만큼 다양한 Product을 반환한다. Client는 최종적으로 반환하는 Product을 조금 더 유연하게 생성하기 위해 Builder Pattern을 적용한다.

- Purpose
  - Allows for the dynamic creation of objects based upon easily interchageable algorithms
- Use When
  - Runtime control over the creation process is required
  - Multiple representations of creation algorithms are required
  - Object creation algorithms should be decoupled from the system
  - The addition of new creation functionality without changing the core code is necessary
- Comparison with Abstract Factory
  - Builder constructs the object **step-by-step** and the **result is requested at a later stage**
  - Builder Pattern focuses only the result object
  - Abstract factory returns the requetsted object immediately
  - Abstract factory does not have an abstract builder; application calls the factory method directly

## Builder Pattern Structure

- Director knows what parts are needed for the final product
- Concrete builder knows **how to produce the part** and **add it** to the final product

<img src="/_img/2022-09-28/builder pattern structure.png">

- Client
  - selects director and concrete builder to build the product
  - asks concrete builder to return final constructed product
- Director 
  - knows what steps it takes to build a product
  - but it does not know how each step is to be carried out 
- Builder
  - specifies an abstract interface for creating parts of a Product object 
- Concrete Builder
  - constructs and assembles parts of the product by implementing the Builder interface 
  - defines and keeps track of the representation it creates
  - provides an interface for retrieving the product
- Product
  - represents the complex object under construction

## When a Builder Shouldn't be Used

- If the interface is not stable the Builder has few benefits
  - Every interface change requires a change to the Controller and impacts the abstract base class or its objects
  - A new method would require changing the base class and all concrete classes that will need to override the new method
  - A specific method interface change would require all concrete classes supporting the old method to change

