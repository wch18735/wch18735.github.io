---
title: "[Design Pattern] Factory Pattern (팩토리 메소드, 추상 팩토리 패턴)"
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

## Abstract Factory Pattern

Abstract Factory Pattern은 관련된 객체들의 집합을 생성하기 위해 사용되는 인터페이스를 제공한다. Families of products를 생성하거나 연관된 Family가 특정한 테마를 가질 때 유용하게 사용할 수 있다. 위 예시에 대입하자면 피자(Product)는 Family(도우, 소스)을 가지는데, 이들이 특정한 테마(이탈리안 피자, 아메리칸 피자)에 종속되어 종류가 바뀌는 경우(얇음/두꺼움, 토마토/크림) 사용할 수 있다.


### Example of abstract factory pattern with pizza stores

아래는 간단히 피자를 이용한 Abstract Factory Pattern 적용 예시이다. 먼저 도우와 소스 인터페이스를 정의한다.

```java
public interface Dough {
    String getDescription();
}

public interface Sauce {
    String getDescription();
}
```

그리고 이를 구현하는 클래스들을 정의한다.

```java
public class ThinCrustDough implements Dough {
    public String getDescription() {
        return "Thin Crust Dough";
    }
}

public class ThickCrustDough implements Dough {
    public String getDescription() {
        return "Thick Crust Dough";
    }
}

public class TomatoSauce implements Sauce {
    public String getDescription() {
        return "Tomato Sauce";
    }
}

public class CreamySauce implements Sauce {
    public String getDescription() {
        return "Creamy Sauce";
    }
}
```

그 다음, PizzaStore를 위한 Abstract Factory 인터페이스를 정의한다. 이 인터페이스는 도우와 소스 인터페이스를 반환하도록 정의한다.

```java
public interface PizzaIngredientFactory {
    Dough createDough();
    Sauce createSauce();
}
```

이렇게 Factory가 만들어졌다면, 구체적인 팩토리를 구현한다. 아래 코드에서 이탈리안, 아메리칸 피자 타입에 맞춰 재료를 생성하는 클래스들이 구현된 것을 확인할 수 있다. 이렇게 만들어진 **구체적인 팩토리 클래스**는 추후 **추상적인 팩토리 인터페이스**를 내부 속성으로 가진 클래스가 초기화 될 때 전달되어, 특성에 맞는 값들을 반환해주게 된다.

```java
public class ItalianPizzaIngredientFactory implements PizzaIngredientFactory {
    public Dough createDough() {
        return new ThinCrustDough();
    }

    public Sauce createSauce() {
        return new TomatoSauce();
    }
}

public class AmericanPizzaIngredientFactory implements PizzaIngredientFactory {
    public Dough createDough() {
        return new ThickCrustDough();
    }

    public Sauce createSauce() {
        return new CreamySauce();
    }
}
```

여기까지 만든 다음 Pizza 클래스와 이를 상속하는 치즈 피자를 정의한다. `PizzaIngredientFactory` 라는 추상 팩토리가 치즈 피자 안에 존재하는 것에 주목하자. 이것이 Abstract Factory의 핵심으로, 종류에 따라 구체화된 추상 팩토리를 전달 받기만 하면, 언제든 같은 행위에 대해서도 서로 다른 속성을 부여할 수 있게 된다. 가령, 전달되는 `ingredientFactory`가 이탈리안인지 아메리칸인지에 따라 서로 다른 종류의 도우와 소스를 전달받을 수 있다는 것이다.

가령 치즈 피자 클래스가 처음 생성될 때, 아메리칸 재료를 반환하는 클래스를 인자로 받아 팩토리에 설정해 놓으면, 아래 `ingredientFactory.createDough()`, `ingredientFactory.createSource()`는 구현에 맞게 항상 두껍고, 크리미한 소스를 반환해준다.

```java
public abstract class Pizza {
    String name;
    Dough dough;
    Sauce sauce;

    public abstract void prepare();

    public void bake() {
        System.out.println("Baking " + name);
    }

    public void setName(String name) {
        this.name = name;
    }
}

public class CheesePizza extends Pizza {
    PizzaIngredientFactory ingredientFactory;

    public CheesePizza(PizzaIngredientFactory ingredientFactory) {
        this.ingredientFactory = ingredientFactory;
    }

    public void prepare() {
        System.out.println("Preparing " + name);
        dough = ingredientFactory.createDough();
        sauce = ingredientFactory.createSauce();
    }
}
```

마지막으로 위에서 배웠던 팩토리 메소드 패턴을 적용한다. 피자 스토어에서는 `orderPizza(String type)`에 따라 서로 다른 종류의 피자를 제공한다. 이전에는 모두 같은 재료를 사용한다고 가정했으나, 피자 스토어가 다양해지며 재료도 다양해졌다. 그래서 재료의 다양성을 적용하기 위해 Abstract Factory 패턴을 적용해 서로 다른 재료를 전달할 수 있게 되었다.

```java
public abstract class PizzaStore {
    public Pizza orderPizza(String type) {
        Pizza pizza = createPizza(type);
        pizza.prepare();
        pizza.bake();
        return pizza;
    }

    protected abstract Pizza createPizza(String type);
}

public class ItalianPizzaStore extends PizzaStore {
    protected Pizza createPizza(String type) {
        Pizza pizza = null;
        PizzaIngredientFactory ingredientFactory = new ItalianPizzaIngredientFactory();
        if (type.equals("cheese")) {
            pizza = new CheesePizza(ingredientFactory);
            pizza.setName("Italian Cheese Pizza");
        }
        return pizza;
    }
}

public class AmericanPizzaStore extends PizzaStore {
    protected Pizza createPizza(String type) {
        Pizza pizza = null;
        PizzaIngredientFactory ingredientFactory = new AmericanPizzaIngredientFactory();
        if (type.equals("cheese")) {
            pizza = new CheesePizza(ingredientFactory);
            pizza.setName("American Cheese Pizza");
        }
        return pizza;
    }
}

```

이로써, 다른 지역에 PizzaStore가 생기게 된다면, 단순히 해당 지역의 재료를 제공하는 재료 팩토리만을 만들면 된다. 그치만 한 가지 변경에 취약한 부분으로는 재료가 추가되는 경우 상위 인터페이스부터 변경이 발생한다는 점이다.

그렇기 때문에 처음은 생성의 유용성을 주면서 동시에 구현도 간단한 Factory Method 패턴을 시작으로 하고, 시스템 요구사항을 분석하며 필요에 따라 Abstract Factory, Prototype, Builder 패턴으로 넘어가는 것을 권장하고 있다.

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