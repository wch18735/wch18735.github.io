---
title: "[Java] 상속 (Inheritance)"
excerpt: "inheritance example"
date: 2022-01-23
layout: single
classes: wide
category:
    - java
tag:
    - inheritance
author: 1FeS
comments: true
---

<span style="font-size:1.5em; font-weight:bold;">인터페이스 (Interface) 예제</span>

```java
interface Animal {
    public void animalSound();
    public void run();
}

interface Pet {
    public void follow();
}

class Dog implements Animal, Pet {

    private String name;

    public Dog(String name){
        this.name = name;
    }

    @Override
    public void animalSound() {
        System.out.println("Bark");
    }

    @Override
    public void run() {
        System.out.println("Run " + this.name);
    }

    @Override
    public void follow() {
        System.out.println(this.name + " follow master");
    }
}

class Test_Interface {
    public static void main(String[] args) {
        Dog dog = new Dog("Angela");

        dog.animalSound();
        dog.run();
        dog.follow();
    }
}
```

<br/>
<span style="font-size:1.5em; font-weight:bold;">상속 (Inheritance) 예제</span>

```java
import PP.Imported;

class A {
    public A(){
        System.out.println("A created");
    }

    public void doSomething(){
        System.out.println("A print!");
    }
}

class B extends A{
    public B(){
        System.out.println("B created");
    }

    public void doSomething(){
        System.out.println("B print!");
    }
}

class C extends B {
    public C(){
        System.out.println("C created");
    }

    public void doSomething(){
        System.out.println("C print!");
    }
}

public class Test_Interitance {
    public static void main(String[] args) {
        Imported im = new Imported();

        C c = new C();
        c.doSomething();
    
        A a = new C();
        a.doSomething();
    }
}
```

결과:

```sh
Imported Created
A created
B created
C created
C print!
A created
B created
C created
C print!
```

즉, `클래스 C` 생성자를 가장 상위 `클래스 A` 변수에 대입할 때, `클래스 C` 가 `클래스 A` 를 상속하고 있기에 자동으로 `doSomething()` 메소드 주소가 `C 클래스.doSomething()` 을 가리킨다. 반대 경우는 Error 가 발생한다.

<br/>
<span style="font-size:1.5em; font-weight:bold;">Package 활용</span>

- 폴더 이름을 PP 로 지정 (Package 명)
- 해당 폴더 내 소스파일에 `package PP;` 이용해 어떤 패키지에 포함되는 소스인지 명시
- 해당 소스파일 내 클래스를 패키지 외부에서 접근할 수 있도록 바꾸려면 `public` 수정자를 지정

```java
package PP;

public class Imported {
    public Imported(){
        System.out.println("Imported Created");
    }
}
```