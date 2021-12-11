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

기본적으로 *Interface* 구현하거나 *Class* 를 상속하는 특정 개체는 생성시에 내부 메소드가 동적으로 바인딩 된다. 내 생각으로는 이 과정이 의존성 주입을 가능케하는 기술적 요소라 생각한다.

예를들어 `인터페이스 Example` 을 구현하는 아래 코드를 살펴보자.

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
```

코드를 실행하면 결과는 다음과 같다.

```sh
run example!
run example! twice!
```

보다시피 동적인 바인딩이 일어난다. 같은 인터페이스를 구현하더라도 `ExampleImp` 와 `ExampleImp2` 둘 중 어느 클래스를 생성하느냐에 따라 기능이 새롭게 정의된다. 여기서 `인스턴스를 생성해 전달`해서 사용하는 것을 `주입`이라고 생각해보자. 

*Spring* 에서의 의존성 주입은 이렇게 사용자가 직접 하나하나 코드를 작성해 변경하는 것보다 외부 설정 파일에서 `어떤 클래스를 생성해 전달`하느냐를 결정할 수 있게 해주는 것이라고 생각하면 편할 것 같다.

여기까지 이해하고 아래 Dependency Injection 을 읽어보면 이해가 쉬울 듯.

# **DI (Dependency Injection)**

**DI (Dependency Injection)** 이란 스프링만의 다른 Framework 과 차별화되는 점이다.

> DI (Dependency Injection) 이란? <br/>
객체를 직접 생성하는 게 아니라 `외부에서 생성한 후 주입` 시키는 디자인 패턴을 뜻한다. <br/>
두 객체 간의 관계를 외부에서 결정하고, 인터페이스를 사이에 두어 클래스 레벨에서는 의존 관계가 고정되지 않도록 한다. 나아가 런타임에 두 객체 간 관계를 다이나믹하게 주입하여 유연성을 확보하고 결합도를 낮출 수 있게 해준다.

이러한 의존성 주입 방법에는 아래와 같은 방법이 있다.

- Constructor Injection (생성자 주입)
- Setter Injection (수정자 주입)
- Field Injection (필드 주입)

## Constructor Injection (생성자 주입)

- 생성자의 호출 시점에 1회 호출되는 것이 보장
- 주입받은 객체가 변하지 않으며, 반드시 객체 주입이 필요한 경우 강제하기 위해 사용
- 생성자가 한 개만 있을 경우 `@Autowired` 생략해도 주입 가능하도록 편의성 제공

## Setter Injection (수정자 주입)

- 필드 값을 변경하는 `Setter` 를 통해 의존 관계 주입하는 방법
- 주입하는 객체가 변경될 가능성이 있는 경우 사용
- `@Autowired` 로 주입할 대상이 없는 경우에 오류가 발생하며, 주입 대상 없이 동작하도록 하려면 `@Autowired(required=false)` 통해 설정 할 수 있음

## Field Injection (필드 주입)

- 필드에 의존 관계를 주입하는 방법
- 외부에서 변경이 불가능하다는 단점이 존재해 테스트 코드 중요성이 부각됨에 따라 거의 사용하지 않게 되었음

## Bean 컨테이너

스프링에서는 객체를 Bean 이라고 부른다. 그리고 프로젝트가 실행될 때, 프로젝트 내에서 Bean 으로 사용하는 객체의 생성 및 소멸 관련 작업을 자동으로 수행하는 곳이 있다. 이 곳을 Bean 컨테이너라고 부른다.

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