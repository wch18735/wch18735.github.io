---
title: "[SW Engineering] Object-Oriented Programming"
excerpt: "object oriented programming introduction"
date: 2022-08-22
layout: single
classes: wide
category:
    - software engineering
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

# Object-Oriented Programming

Object-Oriented Programming 이란 절차지향 프로그래밍과는 반대되는 개념으로, 컴퓨터 프로그램을 명령어의 순차적 실행으로 보는 시각에서 벗어나 객체들간의 상호작용으로 나타내고자 하는 노력의 산물이라 할 수 있다. 이 프로그래밍 패러다임은 소프트웨어를 유연하고 변경이 쉽게 만들고, 개발과 보수를 간편하게 하며, 직관적 코드 분석을 가능하게 한다는 장점이 있다. 

그렇다면 객체지향 프로그래밍에서 뜻하는 객체란 무엇일까. 객체가 무엇인지 모르는데, 객체지향 프로그래밍을 할 수는 없기 때문에 객체에 관해 조금 더 자세히 알아보아야 한다. 

객체란 `An object represents an entity` 라는 간단한 말로 설명된다. 객체는 물리적이거나 개념적인, 나아가 추상적인 어떠한 사상 등 상태와 행위를 가지는 모든 것을 뜻한다. 트럭, 화학 작용, 색깔 등 특정한 상태나 그 개념이 동작하거나 상호작용하는 특정한 방법만 있다면 모두 객체가 될 수 있다.

> An object is an entity with a well-defined boundary and identity that encapsulates state and behavior.

이를 [Oracle Java - object](https://docs.oracle.com/javase/tutorial/java/concepts/object.html) 에서는 아래와 같이 보여준다. Object 모식도 안에는 객체가 가진 상태(State)가 있고, 바깥쪽에는 객체가 어떻게 행동하는지, 상호작용하는지를 묘사하는 행위(Method)가 있다. 

<img src="/_img/2022-08-22/concepts-object.png">

## 객체 상호작용 (Object collaboration) : Sequence Diagram

절차지향 언어와 객체지향 언어의 큰 차이 중 하나를 꼽자면 데이터 접근성이 있다. 절차지향 언어는 Global 하게 지정된 데이터에 논리 블록인 함수들이 Public 하게 접근할 수 있다. 절차지향 언어는 적절한 데이터 구조와 알고리즘을 적용해 요구사항을 만족하도록 기능하는 것이 핵심이라고 볼 수 있다. 이것은 마치 알고리즘 문제 하나를 해결하는 것과 비슷하다고 생각할 수 있다.

이와 반대로 객체지향 프로그래밍은 데이터에 쉽게 접근할 수 없다. 위에서 보았던 그림에서 객체 바깥 쪽이 얇은 막에 둘러싸인 것처럼 테가 그려진 것을 볼 수 있다. 이는 객체 내부 상태에는 직접 접근할 수 없음을 나타내는 것인데, 객체 Principle 중 하나인 Encapsulation 이라고 한다. 이렇게 데이터에 직접 접근하는 것을 막음으로써 데이터의 무분별한 수정을 막을 수 있고, 개발자에게 객체 내부 정보를 보이지 않게 할 수 있다.

<img src="/_img/2022-08-22/OOP vs Procedural Programming.png">

그럼 객체는 어떻게 상호작용 하는가. 객체는 자기 내부 상태 정보에 직접 접근을 통제하는 대신 Public 하게 공개된 Method 를 통해 데이터를 전달한다. 

예를 들어, Professor 객체가 Student 의 ID 값이 필요하다고 가정하자. Professor 객체는 `Student.ID` 와 같은 방법으로 ID를 받을 수 없다. 그 대신 Professor 는 Student 의 `getID()` 메소드를 통해 ID 값을 반환받음으로써 문제해결에 필요한 정보를 얻을 수 있다.

이처럼 특정한 상황에서 서로 다른 객체는 각자의 정보가 필요할 때가 있다. 이렇게 특정한 상황을 가정한 시나리오 상에서 객체가 어떻게 상호작용 하여 문제를 해결하는지를 나타낸 것을 **Sequence Diagram** 이라고 한다. 아래는 <u>강사가 학생의 시험 성적을 입력하고, 100점일 때는 특급점수를 추가한다</u>는 하나의 시나리오를 보여주는 Sequence Diagram 이다.

<img src="/_img/2022-08-22/exam-sequence-diagram.png">

## 객체지향 프로그래밍을 위해

어떤 분야든 단순히 아는 것과 실제로 경험하는 것의 차이는 크다. 좋은 객체지향 프로그래밍을 위해서는 요구사항 분석에서부터 객체지향 프로그램 설계, 개발과 배포, 유지 및 보수에 걸친 모든 범위를 수행해보는 것이 중요하다. 그리고 그 과정에서 나만의 깨달음을 하나씩 쌓아 올리다보면 의식하지 않아도 좋은 좋은 소프트웨어를 만드는 엔지니어가 되어있지 않을까 생각한다. 

다음은 Object-Oriented Principles 를 공부해보고 내 개발 경험에 어떻게 녹일 수 있었을까하는 자조섞인 글을 써볼까 한다.