---
title: "[SW Engineering] Object-Oriented Principles"
excerpt: "basic principles of Object-Oriented"
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

# Basic Principles of Object-Oriented

객체지향에서 추구하는 다섯 가지 원칙이 있다. 잘 설계된 객체라면 가지고 있어야 할 성질이라고 생각하면 좋을 것 같다. 간단하지만, 기억하려고 하면 쉽사리 떠오르지 않아 많은 사람들이 면접을 준비할 때 다시 한 번 공부하게되는 것들 중 하나다. 처음 공부할 때는 **Abstraction**, **Encapsulation**, **Inheritance**, **Polymorphism**, **Composition** 다섯 가지가 각각 추상화, 캡슐화, 상속, 다형성, 구성으로 대응되며, 어떤 특징들을 설명하는지 이해하느라 애썼던 기억이 있다. 이번에는 조금 느슨하게 개념을 풀고, 내 간단한 경험을 녹여 기억하는데 도움을 줘보려고 한다.

## Abstraction : 추상화

추상화를 우리가 이해하기 직관적인 단어로 대체한다면 모델링(Modeling)이라 할 수 있을 것 같다. [Dictionary of Object Technology, 1995](https://www.amazon.com/Dictionary-Object-Technology-Definitive-Reference/dp/1884842097) 책에서는 Abstaction 을 이렇게 설명하고 있다.

> Any model that includes the most important, essential or distinguishing aspects of something while suppressing or ignoring less important, immaterial, or diversionary details. The result of removing distinctions emphasize commonalties.

즉, 소프트웨어 공학에서의 추상화는 하려는 일이나 설명하려고 하는 개체 집합의 공통적 속성은 부각시키고 나머지들은 억제하거나 지우는 것을 뜻한다. 이렇게 추상화된 클래스들을 모식화하여 나타낸 것을 **Class Diagram** 이라 하는데, 그 예시는 아래와 같다.

<img src="/_img/2022-08-22/uml-class-diagram-example-sales-order-system.png">

내가 맨 처음 클래스 다이어그램 개념을 들었을 때엔 어떻게 이걸 한 번에 생각해 그릴 수 있을까란 생각을 했다. 그러나 이런 의문을 가지면서도 클래스 다이어그램은 한 번에 그려지지 않을 것이란 생각은 해보지 않았다. 실제로는 어떨까? 클래스 다이어그램은 한 번에 그려지지 않는다.

PI(Process Innovation)라 불리는 요구사항 추출 과정을 통해 정리된 Use-case 들을 하나하나 해결해 나가기 위해 소프트웨어 엔지니어들은 Sequence Diagram 을 그린다. 시나리오가 요구하는 기능을 구현하거나 문제를 해결하기 위해 그려진 Sequence Diagram 을 보며 Class Diagram 을 하나하나 채워나가는 것이다.

최근 사내 시스템의 Class Diagram 을 볼 기회가 있었다. 생각했던 것보다 많은 클래스들이 복잡하게 연결되어 있었다. 처음은 어마어마하게 많은 양이어서 놀랐고, 소스코드에 잘 녹여져있어서 또 한 번 놀랐던 기억이 있다. 물론 가장 놀라웠던 점을 꼽으라면, 역시 Legacy System 이어서 한창 개발되었던 2011년 주석도 있었다는 것이 기억에 난다.

## Encapsulation : 캡슐화

캡슐화는 내부적으로 어떻게 동작하는지 몰라도 잘 쓸 수 있게 만드는 것으로 마치 자동차 내부 엔진이 어떻게 동작하는지 몰라도 핸들만 조향할 줄 알면 운전을 할 수 있는 것에 비유할 수 있다. 캡슐화는 아래와 같이 설명되며 객체 정보 은닉이라는 특징을 나타나게 한다.

> Design, produce and describe software so that it can be easily used without knowing the details of how it works.

예를 들어 간단한 예금관리 시스템을 만든다고 해보자. 무분별하게 Account 객체 내부 정보에 접근할 수 있다면, 개발자가 소스코드에 `Account-Object.account` 또는 `Account-Object.deposit` 정보를 가져오거나 변경해 마음대로 조작할 수 있게 된다. 그렇기 때문에 심지어 개발자에게도 정해진 Interface 만을 제공해 보다 안전하고, 안정적인 소프트웨어를 구축할 수 있게 할 수 있다.

실제로 보안에 민감한 기능의 경우, SDS에서 자체적 구축한 라이브러리를 제공해 개발을 시키는 경우도 있다고 들었다. 아직 경험이 많지 않아 이런 경우는 보지 못 했는데, 기회가 되면 한 번 확인해보고 싶다.

## Inheritance : 상속

상속이란 객체들의 *is a kind of* 또는 *is-a* 관계를 나타낸다. 상속은 클래스들을 구조화하는데 사용되는데, 공통적인 속성일수록 부모 클래스에 속하며, 특화 기능이나 성질일수록 자식 클래스에 속하게 된다. 이로써 객체를 추가하거나 삭제하는데 용이한 형태로 소프트웨어를 관리할 수 있다.

예를 들어 Animal 클래스를 부모 클래스로 하는 두 클래스 Dog, Lion 을 보자. 각각은 공통적으로 color 속성과 eat() 메소드를 가지고 있다. 그러나 Dog 는 owner 속성에 주인 이름 정보를 저장할 수 있지만, Lion 은 주인 대신 jungleName 이라는 속성에 자신이 속한 정글 이름을 저장할 수 있다. 또한 Dog 는 bark() 메소드로 울음소리를 내고, Lion 은 roar() 로 울음소리를 내는 것을 확인할 수 있다.

<img src="/_img/2022-08-22/oop-inheritance.png">

상속은 어떻게 사용하면 좋은가에 대해 많은 사람들이 저마다의 답을 가지고 있을 것 같다. 나는 내가 봤던 Legacy System 에서 배운 교훈을 적어보려고 한다. 

> 사용자도 자기들이 뭘 원하는지 모른다. 컨펌 받았다고 끝이 아니다. 결국 불편하다고 하면 바꿔줘야 한다. 그러니 뭐든지 변할 수 있다고 생각하고 설계하자

레거시 시스템의 시작은 모든 사업부가 공통적으로 사용하는 기능들과 특화 프로세스가 나뉘어 잘 구현되었으나, 그 끝은 사업 특성상 요구사항이 점차 세분화되며 사용자 니즈가 반영되다보니 덕지덕지 수정된 코드가 붙어 거대해진 말로를 걸었다. 절대적으로 공통적인 영역은 없다는 가정 하에 설계했다면 추후 특화프로세스 반영도 수월하게 이루어졌을 것 같다는 생각이 든다.

## Polymorphism : 다형성

다형성은 한 개의 인터페이스에 서로 다른 여러 구현을 작성하여 Runtime 동안에도 상황에 따라 기능이 달리 동작할 수 있도록 하는 것을 뜻한다. 이는 코드를 작성해 수행해보면 바로 이해가 쉽다.

```java
interface Example { public void run(); }

class ExampleImpA implements Example {
    public void run(){ System.out.println("run A"); }
}

class ExampleImpB extends Example {
    public void run(){ System.out.println("run B"); }
}

class DynamicBindingTest{
    public static void main(String[] args) {
        Example e_A = new ExampleImpA();
        e_A.run();

        Example e_B = new ExampleImpB();
        e_B.run();
    }
}

// :RESULT:
// run A
// run B
```

Object-Oriented Programming Language 에서는 Interface 를 구현하거나 Class 를 상속하는 특정 개체는 생성시에 내부 메소드가 동적으로 바인딩 된다. 이런 특징 덕분에 Dependency Injection 과 같이 외부에서 객체를 생성해 주입하는 디자인 패턴을 적용할 수도 있다. 

동일한 인터페이스를 구현하는 다양한 구현체들을 외부에서 삽입해 사용할 때, 이러한 Polymorphism 성질이 유연하고 간결한 코드 작성과 테스트 용이성 측면에서 많은 도움을 준다. 

## Composition : 구성

사실 내가 면접 준비를 할 때에는 Composition 개념은 빼놓고 공부했었다. 나중에 입사 후 S직군 동기와 커피 한 잔 하며 얘기 나누던 중 [Banana Gorilla Jungle Problem in OOP](https://medium.com/codemonday/banana-gorilla-jungle-oop-5052b2e4d588)와 관련해 얘기를 나누게 되었다. 

간단하게 정리하면, *is-a* 또는 *is a kind of* 관계만으로는 요구사항은 만족시킬 수 있으나 부가적이며, 필요없는 기능까지 추가된 **Waddle Software** 가 나타날 수 있다는 것. 이는 *has-a* 관계를 추가해 해결할 수 있는데, 이를 Composition 이라 하는 것이다.