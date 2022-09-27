---
title: "[Design Pattern] Factory Pattern"
excerpt: "factory method pattern & abstract factory pattern"
date: 2022-09-27
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

# Factory Pattern

- Creational Patterns
    - Allow to creating new objects without explicitly using the new operator
    - We can instantiate different objects without modifying client code
    - Examples: Factory Method, Abstract Factory, Singleton, Builder, Prototype
- Factory Method uses inheritance to decide the object to be instantiated, while Abstract Factory delegates object creation to a factory object
- Factory Method pattern uses inheritance, while Abstract Factory pattern uses composition & delegation
- Abstract factory pattern is one level of abstraction higher than the factory method pattern

## Motivation of creational patterns

OOP에서는 실제 Concrete Class를 사용하는 것보다 Interface를 이용하는 것을 권장하고 있다. 이는 책임을 위임하여 의존성을 낮추고, Polymorphism을 통해 코드 수정 및 설계 변경에 유연하게 대응할 수 있도록 해준다. 그러나 동작의 관점에서가 아닌 객체 생성의 관점에서, 아래와 같이 한 개의 인터페이스를 구현하는 서로 다른 객체들이 동시에 코드 상에 분기되어 나타나는 경우는 Interface를 적용하는 것만으로는 코드 수정으로부터 자유로워지지 않는다.

```java
Duck duck;
if(picnic)
    duck = new MallardDuck();
else if (hunting)
    duck = new DecoyDuck();
else if (inBathTub)
    duck = new RubberDuck();
```

## Pizza Business

아래 좌측은 피자 스토어에서 특정한 타입에 따른 피자를 결정해야 하는 코드이다. 해당 코드에서 변경이 자주 발생하는 **타입에 따른 생성** 코드를 한 데 묶어 팩토리를 만드는 것이 Factory Pattern의 기본 사상이다. 우측은 이렇게 생성하는 역할을 Client 코드에 남겨 놓지 않고 위임하는 팩토리 사상을 적용해 타입에 따라 피자를 생성해 전달하는 형태로 구현한 것이다.

<img src="/_img/2022-09-27/pizza store code.png">

### Simple Factory Pattern (not official)

<img src="/_img/2022-09-27/simple pizza factory.png">

## Factory Method Pattern

앞서 확인한 예제에서는 PizzaStore 종류는 한 가지였다. 여기서 문제가 확장되어 NewYork이나 Chicago와 같은 도시마다 PizzaStore가 존재하고 각 Store마다 스타일이 다르다면 이를 반영하기 위해 어떻게 구조가 설계되어야 할까.

이러한 변화를 적용하기 위해 Factory Method Pattern을 사용 할 수 있다. Factory Method Pattern은 Framework와 같이 코드의 흐름은 미리 정의된 상태에서 사용자가 지정해야 할 부분만을 남겨 우리에게 제공한다.

- Purpose
  - Exposes a method for creating objects, allowing subclasses to control the actual creation process
- Use When
  - A class will not know what classes iit will be required to create
  - Subclasses may specify what objects should be created
  - Parent classes wish to defer creation to their subclasses

핵심은 **protected** 접근 지시자를 가진 **createPizza(String type)** 메소드다. Factory Method Pattern은 상속을 통해 구현되는데, PizzaStore를 상속하는 객체들은 생성하는 메소드를 직접 구현하여 사용해야 한다. 각 도시의 피자 가게들은 가게의 스타일마다 생성하는 코드를 직접 작성해 사용해야 한다.

```java
public abstract class PizzaStore {
    public Pizza orderPizza(String type){
        Pizza pizza;
        pizza = createPizza(type);

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        
        return pizza;
    }

    protected abstract Pizza createPizza(String type);
}
```

위 코드는 피자를 준비하는 서비스를 제공하는 객체 지향 프레임워크의 예제다. 이 프레임워크는 특정한 피자를 준비하는 팩토리 메소드를 제공한다. 프레임워크의 클라이언트는 이를 상속하는 서브 클래스에게 서브 클래스가 구현해야 하는 것을 제공하고 나머지는 모두 구현해 놓는다.

### Ordering a pizza using the Factory Method

1. Need an insatnce of PizzaStore
2. Call orderPizza()
3. orderPizza() calls createPizza()
4. orderPizza() now prepare, bake, cut and box the pizza

<img src="/_img/2022-09-27/factory method pattern.png">

이러한 Factory Method는 Dependency Inversion Principle과 관련이 깊다. 기존에 조건문으로 분기된 새로운 피자들을 생성하는 피자 가게 클래스는 각 피자들을 의존하고 있다. 그러나 Pizza Class를 하나 뽑아내면 Concrete Pizza Class들의 의존성이 모두 Pizza를 향하게 된다.

참고하자면, 위와 같이 Inheritance를 사용하기 때문에 어떤 클래스를 사용할 것인지 Run-time이 아닌 Compile-time에 결정한다고 얘기할 수 있다.

## Abstract Factory Method Pattern

Abstract Factory Pattern은 Families of products를 생성할 때, 연관된 Family가 특정한 테마를 가질 때 유용하게 사용할 수 있다. Abstract Factory Pattern은 대상 Product와 이를 사용하는 클라이언트를 분리할 수 있으며, Product들 관계의 일관성을 유지할 수 있고, Concrete Factory들(새로운 제품군)을 쉽게 추가할 수 있다는 장점이 있다. 그러나 AbstractFactory 안에 새로운 제품을 추가하는 것은 어렵다는 단점이 있다.

그렇기 때문에 처음은 생성의 유용성을 주면서 동시에 구현도 간단한 Factory Method 패턴을 시작으로 하고 Abstract Factory, Prototype, Builder 패턴으로 넘어가는 것을 권장하고 있다.

- Purpose
  - Provide an interface that delegates creation calls to one or more concrete classes in order to deliver specific objects
- Use When
  - The creation of objects should be independent of the system utilizing them
  - Systems should be capable of using multiple families of objects
  - Families of objects must be used together
  - Libraries must be published without exposing implementation details
  - Concrete classes should be decoupled from client

### Abstract Factory Pattern structure

- Abstract Factory
  - Declares an interface for operations that create abstract product objects
- Concrete Factory
  - Implements the operations to create concrete product objects
- Abstract Product
  - Declares an interface for a type of product object
- Concrete Product
  - Defines a product object to be created by the corresponding concrete factory
  - Implements the Abstract Product interface
- Client
  - Uses only interfaces declared by Abstract Factory and Abstract Product

<img src="/_img/2022-09-27/abstract factory pattern.png">

### Ordering a pizza using the Factory Method and Abstract Factory

1. Instantiating PizzaStore
2. Order
3. orderPizza() calls createPizza()
  - BEGIN of abstract factory pattern for products(ingredients)
4. createPizza() now needs ingredientFactory
5. prepare() creates ingredients using ingredient factory
  - END of abstract factory pattern for products(ingredients)
6. Now it is ready. orderPizza() calls bake(), cut(), box()