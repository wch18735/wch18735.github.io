---
title: "[Spring] Dependency Injection (의존성 주입)"
excerpt: "dependency injection"
date: 2021-12-11
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - dependency injection
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Dependency Injection

**DI (Dependency Injection)** 이란 스프링 프레임워크에 의해 구현되는 하나의 패턴이다.

> DI (Dependency Injection):
DI란 객체를 직접 생성하는 게 아니라 **외부에서 생성한 후 주입** 시키는 디자인 패턴을 뜻한다.
두 객체 간의 관계를 외부에서 결정하고, 인터페이스를 사이에 두어 클래스 레벨에서는 의존 관계가 고정되지 않도록 하는 방법으로 구체화한다. 이를 통해 런타임에 두 객체 간 관계를 다이나믹하게 주입하여 유연성을 확보하고 결합도를 낮출 수 있게 해준다. 결합도가 낮은 객체끼리는 자연스레 변경에 유연해지게 된다.

## Spring Framework 의존성 주입

스프링 프레임워크에서 의존성 주입은 3가지 방법을 통해 이루어지며, 각각의 특징은 아래와 같다.

- Constructor Injection (생성자 주입)
    - 생성자의 호출 시점에 1회 호출되는 것이 보장
    - 주입받은 객체가 변하지 않으며, 반드시 객체 주입이 필요한 경우 강제하기 위해 사용
    - 생성자가 한 개만 있을 경우 `@Autowired` 생략해도 주입 가능하도록 편의성 제공
- Setter Injection (수정자 주입)
    - 필드 값을 변경하는 `Setter` 를 통해 의존 관계 주입하는 방법
    - 주입하는 객체가 변경될 가능성이 있는 경우 사용
    - `@Autowired` 로 주입할 대상이 없는 경우에 오류가 발생하며, 주입 대상 없이 동작하도록 하려면 `@Autowired(required=false)` 통해 설정 할 수 있음
- Field Injection (필드 주입)
    - 필드에 의존 관계를 주입하는 방법
    - 외부에서 변경이 불가능하다는 단점이 존재해 테스트 코드 중요성이 부각됨에 따라 거의 사용하지 않게 되었음

## 의존성 (Dependency) 정의

의존성이란 연결이다. 가령, 고차원 객체를 상속하거나, 고차원 인터페이스를 구현하는 저차원 객체는 강한 연결을 가지고 있다고 말한다. 또한, 저차원 객체는 고차원 모듈에 대한 의존성을 가지고 있다고 표현할 수 있다. 

반대로, 약한 의존성 관계로 아래 Human - HumanType Association 관계를 예시로 들 수 있다. Human 객체는 HumanType을 가지고 있으며, HumanType에 의존성을 가지고 있으나 상속이나 구현과 같이 강한 의존성은 아니다. 

```java
class Human {
    HumanType htype;

    public setType(HumanType mbti){
        this.htype = mbti.type;
    }
}
```

이렇듯 객체 지향 패러다임에는 다양한 의존성이 존재하며, 객체 지향 소프트웨어를 잘 구축하기 위해서는 이러한 Dependency들을 얼마나 효율적으로 관리하는지에 달려있다고 간주해도 무방하다.

의존성 주입이란 궁극적으로, 앞서 살펴본 의존성이 생성되는 위치를 외부로 가져온다. 앞의 예시에서 확인할 수 있던 것처럼 보통의 의존성은 코드를 작성하는 순간 생성된다. Human 클래스를 작성하는 순간 HumanType에 대한 의존성이 생기는 것이다. 그러나 스프링 프레임워크에서 제공하는 Dependency Injection은 적어도 이러한 Dependency를 Runtime 영역으로 지연시키며 그 위치를 코드 외부로 가져와 한 곳에서 관리할 수 있게 해준다. 생성자 주입, 수정자 주입, 필드 주입과 같은 방법을 통하면 코드 전체적으로 흩어진 상태로 관리되던 의존성을 모두 한 곳에서 관리할 수 있게 된다.

### 의존성과 다형성

Object-Oriented Programming에서 *Interface*를 구현하거나 *Class*를 상속하는 객체는 내부 메소드가 동적으로 바인딩 된다. 우리는 이를 Polymorphism(다형성)이라 부르며, 객체 지향 언어의 중요한 특징 중 하나로 알고있다. 이러한 동적 바인딩이 가능하다는 것을 달리 말하면, 내가 전달하는 인스턴스에 따라 **기능A**라는 메소드를 서로 다르게 동작시킬 수 있다는 것이다. 이러한 성질은 Coupling을 제거해 소프트웨어 유지 및 보수에 큰 도움을 준다.

예를 들어, 아래와 같이 Example 인터페이스를 구현하는 ExampleImp, ExampleImp2를 보자. 사용자는 Example 인터페이스로 객체를 전달받아 동일한 메소드이나 구현만 다른 run()을 호출해 사용할 수 있다. 

```java
interface Example {
    public void run();
}

class ExampleImp implements Example{
    public void run(){
        System.out.println("run example!");
    }
}

class ExampleImp2 extends ExampleImp {
    public void run(){
        System.out.println("run example! twice!");
    }
}

class Dynamipublic{
    public static void main(String[] args) {
        Example e = new ExampleImp();
        e.run();

        Example e2 = new ExampleImp2();
        e2.run();
    }
}

/* 
result:
run example!
run example! twice!
*/
```

보다시피 동적인 바인딩이 일어난다. 같은 인터페이스를 구현하더라도 **ExampleImp**와 **ExampleImp2** 둘 중 어느 클래스를 생성하느냐에 따라 기능이 새롭게 정의된다. 이렇게 **인스턴스를 생성해 외부에서 전달**하는 것을 **주입**이라고 한다. 스프링 프레임워크에서 의존성 주입은 외부 설정 파일에서 **어떤 클래스를 생성해 전달**함으로써 코드 전반에 흩어져 관리되던 의존성들을 한 곳에서 관리하게 만들며, Coupling이 줄어드는 방향으로 관계를 설계하도록 가이드 한다.

## DI (Dependency Injection 구현)

스프링에서는 객체를 Bean 이라고 부른다. 그리고 프로젝트가 실행될 때, 프로젝트 내에서 Bean 으로 사용하는 객체의 생성 및 소멸 관련 작업을 자동으로 수행하는 곳이 있다. 이 곳을 Bean 컨테이너라고 부른다. 아래 코드는 비슷하게 동작하는 BeanFactory를 직접 구현해 테스트해보는 레포지토리이다. 링크 아래 정리된 사항을 주석으로 확인할 수 있다.

- [Dependency Injection Test](https://github.com/wch18735/di-practice)
    - Annotation 선언 방법
    - Reflections 간단한 활용법
    - Map<Class<?>, Object> 구성 과정


## 개인적인 Dependency 관련 용어 정리

- **A가 B를 의존한다 :** A가 B를 멤버 변수나 로컬 변수로 가지고 있거나 파라미터로 전달 받아 B의 메소드를 호출한다.
-  **Component :** 소스코드의 수정 없이 다른 프로젝트에서도 곧 바로 재사용 가능한 모듈. 설계가 제대로 되지 않은 어플리케이션 구조에서는 Leaf 클래스만 재사용 가능한 경우가 더러 발생한다.
- **Dependency Rot (의존성 부패) :** 고차원 모듈이 저차원 모듈을 의존하고, 저차원 모듈이 다시 고차원 모듈을 의존하는 상태
- **Dependency Inversion Principle (DIP) :** 
  -  `고차원 모듈은 저차원 모듈에 의존하면 안 된다.` 
  -  `두 모듈 모두 다른 추상화 된 것에 의존해야 한다.`
  -  `추상화 된 것은 구체적인 것에 의존하면 안 된다.`
  -  `구체적인 것이 추상화 된 것에 의존해야 한다.`
  - **고차원 모듈 :** 특정 모듈을 의존하는 모듈
  - **저차원 모듈 :** 의존 대상이 되는 모듈
- **Dependency Inversion :** 기존 Dependency 방향을 바꾸는 것. 저차원 모듈이 추상화 된 대상을 의존하게 만듦으로써 수행 가능하다.