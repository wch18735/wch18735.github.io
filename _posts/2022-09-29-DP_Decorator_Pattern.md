---
title: "[Design Pattern] Decorator Pattern (장식자 패턴)"
excerpt: "decorator pattern"
date: 2022-09-29
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

# Decorator Pattern

Decorator Pattern은 **이미 존재하는 객체의 책임이나 행동을 변경**하기 위해 사용된다. 보통 기능을 추가하는 경우 Subclass를 만드는 경우가 많은데, 이것이 반복되다보면 클래스 폭발 현상이 발생하곤 한다. 이와 같은 현상을 예방하고 새로운 서비스를 제공하기 위해 Wrapping을 통해 다양한 기능을 제공하는 Decorator Pattern을 이용한다.

- Purpose
    - Allows for the dynamic wrapping of objects in order to modify their existing reponsibilities and behavior
- Use When
    - Object reponsibilities and behaviors should be dynamically modifiable
    - Concrete implementations should be decoupled from reponsibilities and behaviors
    - Subclassing to achieve modification is impractical or impossible
    - Specific funcionality should not reside high in the object hierachy
    - A lot of little objects surrounding a concrete implementation is acceptable

## Decorator Idea

먼저 Decorator를 만들기 위한 한 가지 제약 조건이 있다. 그것은 Decorator가 감싸려는 객체와 같은 super type을 공유해야 한다는 것이다. 아래 그림을 보면 ConcreteComponent를 Decorating 하기 위해 Component라는 super type을 공유하고 있는 것을 확인할 수 있다.

<img src="/_img/2022-09-29/decorator pattern.png">

Component를 상속하는 Decorator는 wrappedObj라는 참조 화살표를 하나 더 가지고 있는데, 이것이 장식자 패턴을 구현하는 핵심이다. 위와 같이 구성되는 장식자 패턴은 다시 한 번 강조하지만, 서브 클래스를 통한 기능 추가로 발생하는 문제를 해결해준다. 

> The Decorator Pattern attaches additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.

## Example: Beverage with condiments

아래와 같이 HouseBlend, DarkRoast, Decaf, Espresso 네 가지 음료가 있고 각 음료는 cost()를 통해 가격을 반환한다. 이 음료들에 Milk, Mocha, Whip, Soy 첨가물을 추가하고 각 첨가물마다 가격을 달리해 주문을 받으려고 한다. 

이 때, 음료당 한 가지 첨가물들만 추가 가능하다고 가정해도 16개 조합이 나올 수 있다. 이들을 각각 클래스로 만들면 16개 클래스를 만들어야 하며, 만약 첨가물들을 넣을 수 있는 개수를 제한하지 않는다면 4 + 16 + 48 + 16 + 4 = 88 가지 클래스를 만들어야 하는, 이른바 클래스 폭발이 발생한다.

<img src="/_img/2022-09-29/decorator example original design.png">

이를 해결하기 위해 가장 먼저 생각할 수 있는 방법으로는 각 첨가물들을 Boolean 형태로 저장해 그 가격을 계산하도록 만드는 것이다. 그렇게 되면 상속을 통해 만드는 Subclass들이 폭발적으로 많아지는 위 문제를 해결 할 수 있을 것이다.

<img src="/_img/2022-09-29/beverage with condiment boolean.png">

그러나 위와 같은 설계는 새로운 첨가물이 추가되거나, 첨가물 가격이 변경될 때마다 모든 코드를 변경해야 한다는 명백한 한계가 존재한다. 또한 새롭게 추가되는 Beverage 종류가 해당 첨가물들과 어울리지 않을 수 있다. 이를 OCP를 만족하도록 설계하기 위한 해법으로 Decorator Pattern이 제공해 줄 수 있다.

아래는 Decorator Pattern을 적용한 클래스 다이어그램의 모습이다. 실제로 코드로 작성할 때는 Beverage와 CondimentDecorator에 abstract 키워드가 붙지만 모두 클래스로 나타나는 점에 유의해서 살펴보자. 또한, package를 나누어 관리해야 그림과 같은 계층을 나타낼 수 있다.

<img src="/_img/2022-09-29/applying to example.png">

위 Main.java 코드는 아래와 같다. 실제로 이를 실행시켜보면 주석으로 남긴 결과가 정확하게 나타난다.

```java
package pattern.decorator.coffee;

import pattern.decorator.coffee.beverage.HouseBlend;
import pattern.decorator.coffee.condiment.Milk;
import pattern.decorator.coffee.condiment.Mocha;

public class Main {
	public static void main(String[] args) {
		Beverage houseBlend = new HouseBlend();
		System.out.println(houseBlend.getDescription());
		System.out.println(houseBlend.cost());
		
		houseBlend = new Milk(houseBlend);
		System.out.println(houseBlend.getDescription());
		System.out.println(houseBlend.cost());
		
		houseBlend = new Mocha(houseBlend);
		System.out.println(houseBlend.getDescription());
		System.out.println(houseBlend.cost());
	}
}

/*
HouseBlend
100
HouseBlend with Milk
110
HouseBlend with Milk with Mocha
122
*/

```