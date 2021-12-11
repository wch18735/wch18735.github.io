---
title: "[Java] Abstraction"
excerpt: "java abstraction"
date: 2021-12-11
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

# Abstraction (추상화)

**Data abstraction** 이란 자세한 부분은 숨기고 사용자에게 필수적으로 제공되어야 할 부분만 보여주는 과정이다.

`abstract` 키워드는 non-access modifier 로 클래스와 메소드 모두 사용될 수 있다.

- **Abstract class :** 오브젝트를 생성할 수 없는 제한된 클래스. 접근하기 위해서는 다른 클래스가 추상 클래스를 상속해야 가능하다.
- **Abstract method :** 오직 `abstract class` 에서 사용될 수 있는 것으로 `Method body` 를 가지지 않는다. `Method body` 는 이를 상속하는 클래스에서 구현된다.


## Abstract Class (추상 클래스)

클래스 앞에 `abstract` 키워드를 붙여준다. 이렇게 만들어진 추상 클래스는 아래 예시처럼 `abstract` 와 `일반 method` 모두를 가질 수 있다.

```java
abstract class Animal {
  public abstract void animalSound();
  public void sleep() {
    System.out.println("Zzz");
  }
}
```

이렇게 정의한 추상 클래스는 `new Constructor()` 를 이용해 생성할 수 없다. 추상 클래스를 사용하려면 `extends` 키워드를 이용해 추상 클래스를 상속하고 추상 메소드를 구현해야 한다.

```java
abstract class Animal {
  public abstract void animalSound();
  public void sleep() {
    System.out.println("Zzz");
  }
}

class Pig extends Animal {
  public void animalSound() {
  System.out.println("The pig says: wee wee");
  }
}

class Main {
  public static void main(String[] args) {
    Pig myPig = new Pig();
    myPig.animalSound();
    myPig.sleep();
  }
}
```

## Interface (인터페이스)

Java 에서 제공하는 또 다른 추상화 방법 중 하나로 `Interface` 가 있다. 인터페이스는 완벽한 추상 클래스로 관계있는 메소드들을 한 곳에 묶어 정의해 놓는다.

추상화 방법 중 하나지만 abstract 대신 interface 를 통해 정의되고, 이를 사용할 때도 extends 대신 implements 키워드를 사용한다. 이때, 여러 개의 인터페이스를 동시에 구현할 수 있다.

```java
interface FirstInterface {
  public void myMethod(); // interface method
}

interface SecondInterface {
  public void myOtherMethod(); // interface method
}

class DemoClass implements FirstInterface, SecondInterface {
  public void myMethod() {
    System.out.println("Some text..");
  }
  public void myOtherMethod() {
    System.out.println("Some other text...");
  }
}

class Main {
  public static void main(String[] args) {
    DemoClass myObj = new DemoClass();
    myObj.myMethod();
    myObj.myOtherMethod();
  }
}
```