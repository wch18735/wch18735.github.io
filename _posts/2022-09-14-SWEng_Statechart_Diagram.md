---
title: "[SW Engineering] Statechart Diagram"
excerpt: "statechart diagram"
date: 2022-09-14
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

# Statechart Diagram

Statechart Diagram은 시스템이 상태 기반으로 동작할 때 그리는 다이어그램이다. 반면 시스템이 비즈니스 기반으로 동작할 때는 Activity Diagram을 그리게 된다. 보편적으로 알고있는 Flowchart Diagram이 Activity Diagram의 가장 흔한 예시인데, 이를 알아보기 전 간단하게 Statechart Diagram을 짚고 넘어가보려고 한다.

Statecart Diagram 이전에 Statecharts라는 Formal Specification, Formal Verification Language가 존재했었다. 이를 UML에서 차용한 것이 Statechart Diagram이며, 기본적으로 FSM(Finite State Machine) 또는 FSA(Finite State Automata)를 뜻한다.

## State

아래 상태 다이어그램에는 시작점과 종료점, 그리고 상태가 동작하는 세 가지 상태를 보여주고 있다. 동작은 상태로 진입하는 시점에 동작하는 entry, 상태가 변화하는 시점에 동작하는 do, 내부 동작을 마친 뒤 상태가 변경되며 동작하는 exit 까지 세 지점에서 발생한다.

<img src="/_img/2022-09-14/statechart diagram basic.png">

State Transition을 조금 더 자세히 표현한 그림은 아래와 같다. 이는 S상태에서 Event가 발생했을 때, Guard를 만족하면 일련의 Action을 수행한 뒤 T로 전환되는 그림을 표현한 것이다.

<img src="/_img/2022-09-14/transition.png" style="width:20em">

간단하 예제를 확인해보자. 아래 그림을 차근차근 따라가보자. 일단 들어올 때, x = 4 값을 가지고 있으며, event를 살펴보면 조건을 만족하기 때문에 x * 2 를 수행하기 위해 S1을 탈출한다. 이 때, exit에 걸린 x++를 수행하기 때문에 5가 되고, 결과적으로 x * 2 를 수행한 값은 10이 된다. 끝으로 entry에 표기된 x++를 수행하여 결과적으로 값은 11이 된다.

<img src="/_img/2022-09-14/transition example.png">

## Composite State

State는 아래 그림과 같이 복잡한 형태로도 그려질 수 있다. 이같이 복잡한 형태를 Composite State라고 하는데, 예제 그림은 상태 내부에 또 다른 상태가 존재할 수 있으며, 같은 상태에서 발생할 수 있는 Event 종류에 따라 분기될 수 있음을 보여주고 있다.

<img src="/_img/2022-09-14/composite state.png">

위 예제를 구현하는 PlantUML 코드는 아래와 같다.

```java
@startuml
state S1
state S2
state S3
 
state S1 {
	[*] --> S1.1
	S1.1 -> S1.2 : e4
	S1.2 --> [*] : e4
}
S1 -> S2
S1.1 -> S2 : e3
S2 --> [*] : e3

S3 -> S1.2 : e1
S3 -> S1 : e2
@enduml
```

## Orthogonal State

State가 동시에 진행되는 것을 표현해주는 방법도 있다. Parallelization Node, Synchronization Node 두 가지를 이용해 동시에 진행되는 상황을 연출 할 수 있다. 동시에 상태가 변화하는 경우 Synchronization Node로 분기가 합쳐진 후에 다음 단계로 진행되는데, 만약 아래 그림에서 State3 내부 동작 후 Synchronization Node로 상태가 변경되지 않는 경우에는 Deadlock이 발생해 프로그램이 멈추게 된다.

<img src="/_img/2022-09-14/orthogonal state.png">

```java
@startuml

state fork_state <<fork>>
[*] --> fork_state
fork_state --> State2
fork_state --> State3

state join_state <<join>>
State2 --> join_state
State3 --> join_state
join_state --> State4
State4 --> [*]

@enduml
```

## History State

어떤 상태에서 일이 진행되었는지를 기록하기 위해 사용한다. 이 때 기록의 종류는 두 가지가 있는데, 각각 Shallow history state와 Deep history state로 나눌 수 있다. **[H]** 로 표현되는 Shallow history state는 기존에 진행되던 부분을 포함하면서 **[H]** 노드가 정의된 곳과 같은 레벨의 State를 찾아 되돌아 간다. 반면 **[H*]** 로 표현되는 Deep history state는 진행되던 상태를 정확하게 찾아가 진행한다.

<img src="/_img/2022-09-14/history state.png">

```java
@startuml
[*] -> State1
State1 --> State2 : Succeeded
State1 --> [*] : Aborted
State2 --> State3 : Succeeded
State2 --> [*] : Aborted
state State3 {
  state "Accumulate Enough Data" as long1
  long1 : Just a test
  [*] --> long1
  long1 --> long1 : New Data
  long1 --> ProcessData : Enough Data
  State2 --> [H]: Resume
}
State3 --> State2 : Pause
State2 --> State3[H*]: DeepResume
State3 --> State3 : Failed
State3 --> [*] : Succeeded / Save Result
State3 --> [*] : Aborted
@enduml
```