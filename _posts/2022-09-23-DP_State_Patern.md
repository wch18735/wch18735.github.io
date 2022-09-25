---
title: "[Design Pattern] State Pattern (상태 패턴)"
excerpt: "state pattern"
date: 2022-09-23
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

# State Pattern

상태 패턴은 기본적으로 내부적인 상태에 따라 같은 자극이 들어와도 다르게 반응해야 할 때 사용된다. 만약 이렇게 외부 자극에 대해 상태에 따라 동적으로 반응하는 형태가 유한하다면, 이는 Statechart Diagram을 통해 효과적으로 설명할 수 있다. Statemachine 개념을 통해 상태의 변화와 동작을 나타낼 수 있는 시스템을 State Pattern을 사용하면 복잡한 분기문이나 조건문 없이 간단하게 나타낼 수 있다.

- State Pattern
    - The State Pattern allows an object to alter its behavior when its internal state changes. The object will appear to change its class.
- Purpose
    - Ties object circumstances to its behavior, allowing the object to behave in different ways based upon its internal state
- Use When
    - The behavior of an object should be influenced by its state
    - Complex conditions tie object behavior to its state
    - Transitions between states need to be explicit

이러한 상태 패턴을 다이어그램으로 나타낸 그림은 아래와 같다. 사용하려는 기능을 상태 인터페이스에 위임하는 것이 가장 큰 특징으로, 어떻게 잠재적 복잡성을 해결하는지를 예시로 알아보고자 한다.

<img src="/_img/2022-09-23/state pattern structure.png">

## State Pattern discovery

<img src="/_img/2022-09-23/gumball01.png">

위 State Diagram은 상태 패턴을 적용하기 위한 예시로, 어렸을 적 동전을 넣어 돌리면 그 안에서 동그란 껌이 나오던 기계의 외부 동작에 따른 상태를 묘사한다. 이를 묘사하기 위해 각 상태를 고정 정수 상수 값으로 선언하는 **final static int** 키워드를 사용해 아래와 같은 클래스를 가장 먼저 떠올릴 수 있다.

```java
public class GumballMachine {
    final static int SOLD_OUT = 0;
    final static int NO_QUARTER = 1;
    final static int HAS_QUARTER = 2;
    final static int SOLD = 3;

    int state = SOLD_OUT;
    int count = 0;

    public GumballMachine(int count){
        // ...
    }

    public void insertQuarter(){
        // ...
    }

    public void ejectQuarter(){
        // ...
    }

    // ...
}
```

외부 이벤트에 따라 정상적으로 동작하는 코드는 쉽게 작성된다. 그러나 언제나 처럼 요구사항이 추가되거나 변경이 일어나게 된다. 한 번 크랭크를 돌릴 때, 확률을 추가하게 되었다고 가정하자.

각 메소드 내부에서 HAS_QUARTER에 해당하는 분기문을 모두 수정해줘야 하는 번거로움이 생긴다. 그리고 이런 일이 반복되면 자연스럽게 내부 분기문도 점차 유지ㆍ보수 복잡도가 상승하게 된다. 

이와 같이 상태에 따라 변경하는 부분이 많아져 난개발로 이어지는 문제를 해결하기 위해 전략 패턴에서 사용했던 사상을 가져와 적용해 볼 수 있다.

- Encapsulate what varies
    - Put each state's behavior in its own class, then every state just implements its own actions
    - The Gumball Machine can delegate to the state object that represents the current state
    - Favor composition over inheritance

새로운 설계는 위 원칙을 현재 문제 상황에 대입해 아래와 같이 진행한다.

- Define State interface that contains a method for every action in the Gumball Machine
- Implement a State class for every state of the machine which is responsible for behavior of the machine when it is in the corresponding state
- Get rid of our conditional code and instead delegate to the state object to do the work for us

이렇게 적용된 클래스 다이어그램은 아래와 같다.

<img src="/_img/2022-09-23/state interface and classes.png">

위와 같은 상태를 사용해 기존 GumballMachine을 아래와 같이 상태를 사용하는 형태로 작성할 수 있다. 기존에 고려했던  conditional statement가 깨끗하게 사라진 것을 확인 할 수 있다.

```java
public class GumballMachine {
    State soldOutState;
    State noQuarterState;
    State hasQuarterState;
    State soldState;

    State state = soldOutState;
    int count = 0;

    public GumballMachine(int nunberGumballs){
        soldOutState = new SoldOutState(this);
        noQuarterState = new NoQuarterState(this);
        hasQuarterState = new HasQuarterState(this);
        sodlState = new SoldState(this);

        this.count = numberGumalls;
        if(numberGumballs > 0) state = noQuarterState;
    }

    public void insertQuarter() { state.insertQuarter(); }
    public void ejectQuarter() { state.ejectQuarter(); }
    public void turnCrank() { state.turnCrank(); }
    public void dispense() { state.dispense(); }
    
    void setState(State state) { this.state = state; }
    void releaseBall(){
        System.out.println("A gumball comes rolling out the slot");
        if(count != 0) count = count - 1;
    }
}
```

이렇게 코드에서 확인 할 수 있는 것처럼 상태를 적용한 설계는 기존 구조를 OCP를 지킬 수 있도록 변화시킨다.

- Changes
    - Localized the behavior of each state into its own class
    - Removed all the troublesome conditional sttatements that would have been difficult to maintain
    - Closed each state for modification, and yet left the Gumball Machine open to extension by adding new state classes
    - Created a code base classs structure that maps much more closely to the Mighty Gumball diagram and is easier to read and understand
- Benefits
    - Puts all behavior associated with a state into one object
    - Allows state transition logic to be incorporated into a state object rather than in a monolithic if or switch statement
    - Helps avoid inconsistent states since state changes occur using just the one state object and not several objects or attributes

## Similarity with Strategy Pattern

상태 패턴과 전략 패턴 다이어그램을 보면 둘이 거의 비슷한 구조를 취하고 있음을 알 수 있다. 둘 모두 Composition과 Delegation을 적용한 예시라는 공통점을 볼 수 있다. 그러나 둘은 해결하고자 하는 문제의 형식이 다르다고 할 수 있다.

- Strategy: the choice of algorithm is fairly stable
- State: a change in the state of the "context" object causes it to select from its "palette" of State objects

상태 패턴은 상태 의존적인 행동을 정보 은닉한다. 이러한 정보 은닉을 통해 행동의 수정이나 상태 추가에 따른 코드 변경을 최소화하고 조건문의 과다한 사용을 방지한다.

반면 전략 패턴은 객체가 가진 알고리즘을 정보 은닉화한다. 이로써 클라이언트 또는 주체 객체가 사용하는 알고리즘을 변경하는 것에 초점을 둔다.

### State Pattern Implementation Issues

- Who defines the state transitions?
    - Option 1: Giving transition responsibility to the Context class fits for simple situations
    - Option 2: Giving transition responsibility to the ConcreteState classes is generally more flexible, but causes implementation dependencies between the Concrete classes

- when are the ConcreteState objects created?
    - Option 1: Create ConcreteState objects as needed
    - Option 2: Create all ConcreteState objects once and have the Context object keep reference to them