---
title: "[SW Engineering] An Introduction to UML"
excerpt: "an introduction to UML"
date: 2022-08-23
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

# An Introduction to UML

통합 모델링 언어(UML, Unified Modeling Language)는 소프트웨어 공학에서 사용되는 표준화된 범용 모델링 언어다. 이는 객체 관리 그룹(OMG, Object Management Group)에서 만든 것으로 소프트웨어 공학 분야에 범용적으로 사용되는 **de facto industry standard** 이다. 

UML과 관련된 더욱 자세한 내용은 [http://www.uml.org/](http://www.uml.org/) 에서 확인할 수 있다. 실제로 사이트에 들어가 UML Specification 문서를 다운받아보면 v2.5.1 기준 796장이 나온다. 모든 내용을 다룰 수는 없기에 UML의 역사나 버전별 차이점과 같은 상세한 내용보다는 주로 사용되는 13개 다이어그램을 [UML - Summary](https://www.tutorialspoint.com/uml/uml_quick_guide.htm) 과 강의를 참고해 정리해보려고 한다.

## UML 2.0 Diagrams

<img src="/_img/2022-08-23/uml_diagram_types.png">

위 사진에서 *글자체가 기울어진* 것은 Abstract Class 를 뜻한다. Diagram, Structure Diagram, Behavior Diagram, Interaction Diagram 등이 Abstract Diagram 으로 하위 다이어그램들의 공통적인 속성을 지니고 있다.

소프트웨어 구조를 설명하는 Structure Diagram 아래에는 Class Diagram, Component Diagram, Object Diagram, Composite Structure Diagram, Deployment Diagram, Package Diagram 이 있다. 반면, 소프트웨어 동작을 모식화하는 Behavior Diagram 아래는 Activity Diagram, Use Case, Diagram, State Machine Diagram 이 있고, 한 개의 Abstract Digram 인 Interaction Diagram 이 붙어있는데, 소프트웨어의 상호작용을 설명하는 Sequence Diagram, Communication Diagram, Interaction Overview Diagram, Triming Diagram 이 이를 구현하고 있다.

OOAD 개발 방법론에 따라 소프트웨어를 개발할 때는 기본적으로 Class Diagram, Sequence Diagram, Use Case Diagram 을 사용한다. 먼저 Use Case Diagram 을 통해 Domain Model 을 도출하고, Sequence Diagram 을 통해 해당 Domain Model 의 Attribute / Method 를 채워가며 Class Diagram 을 완성해 나간다. 이 때, Domain Model 은 Class Diagram Notation 을 사용하기 때문에 위 다이어그램 그래프에 나타나진 않았다.

작성된 Class Diagram 들이 모이면 하나의 Package Diagram 이 되고, 이들이 모여 구현의 단위인 Component Diagram 을 만들게 되는데, 앞서 말한 Class Diagram 이 선행적으로 작성되어야 하기 때문에 Package Diagram 이나 Component Diagram 은 OOAD 뒷단에 나타나며, 어느 정도 모델링이 되고 코드가 작성되어야 이를 확인할 수 있다. 그치만 Component Diagram 까지는 모두 Logical Architecture 를 설명한다는 공통점이 있다. 반면, 하드웨어 상에서 어떻게 동작하는지를 설명하는 Physical Architecture 는 Deployment Diagram 에서 그릴 수 있다. 

Class Diagram 과 함께 사용되는 Object Diagram 이나 Component Diagram 과 함께 사용되는 Composite Structure Diagram 은 필수적이진 않지만 상세 정보를 부가적으로 표현하기 위해 사용되는 다이어그램이다. 

### Use Case Diagram

Use Case Diagram 은 우리 시스템 밖에 있는 외부 사용자(External Actor)가 시스템을 어떻게 사용하는지를 시나리오 기반으로 Text 를 사용해 적는 다이어그램을 말한다. 

<img src="/_img/2022-08-23/use-case diagram example.png">

Use Case Diagram 는 External Actor 와 System Boundary, 그리고 어떻게 사용하는지가 텍스트로 정리된 Use Case 로 구성된다. 여기서 가장 중요한 핵심은 Use Case 를 설명하는 Use Case Descriptor 이다.

<img src="/_img/2022-08-23/Use-case-textual-description-template.png">

Use Case Diagram 은 프로젝트가 진행되며 상세화 정도가 달라지는데, 중요한 것은 요구사항이 변할 수 없다는 것이다. Elaboration Phase 부터 상세화되는 Use Case 들 모두 Template 에 기재된 대체안이나 구성 요소들만을 상세화 대상으로 하며, 핵심이 되는 요구사항은 변경할 수 없는 것이 일반적이다.

### Class Diagram

Class Diagram 은 정적 모델(Static Model)로 시스템을 구성하는 Object 또는 Object Class 가 어떤 Attribute 과 Operation 을 가지고, 어떻게 Communication / Collaboration 하는지를 나타낸다. 

<img src="/_img/2022-08-23/class-diagram-implementation-elements.png">

Static 하다는 뜻을 조금 더 자세히 설명하자면, Class Diagram 에서는 클래스들간의 관계만을 확인할 수 있다는 것이다. 반대로 Dynamic Model 에서는 어떤 시나리오 상황에 어떻게 상세히 동작해 요구 조건을 만족하는지까지 모두 알아낼 수 있다. 이러한 Dynamic Model 의 대표적 예시로는 Sequence Diagram 이 있다.

이러한 Class Diagram 은 Unified Process 기반의 Iterative OOAD 에서는 두 가지 용도로 사용된다. OOA 과정에서는 Domain Model, OOD 에서는 Design class diagram(DCD) 이다.

### Object Diagram

Object Diagram 은 Class Diagram 에 나타난 클래스들 간의 관계에서 특정 시간대, 특정 상황을 뜻하는 **At a specific point of time** 를 자세히 설명할 때 쓰인다. 가령 $1:N$ 관계가 실제로 어떻게 Instanciation 되는지 설명하는 곳에 사용된다.

<img src="/_img/2022-08-23/class diag vs obj diag.png">

### Package Diagram

Package Diagram 은 Class Diagram 을 Packaging 하는 다이어그램이다. Iterative 한 개발 방법론을 따르는 과정에서 나타나는 Class 들을 용도에 따라 묶어주는 역할을 한다. 나아가 클래스 용도가 아닌 특정한 Logical Criteria, 가령 Layered Architecture 라는 논리적 구분을 따르는 방식으로도 그룹화 가능하다.

<img src="/_img/2022-08-23/uml-package-diagram.png">

반면, 실제 소프트웨어 구현과 관련된 Physical Architecture 는 Component Diagram 이나 Deployment Diagram 을 이용해 따로 그려야 한다는 것을 주의해야 한다.

### Component Diagram

Component Diagram 은 실질적으로 소프트웨어를 개발할 떄 사용하는 개발의 단위를 나타낸다. 초기 버전의 UML 에서 Component Diagram 은 물리적인 내용(문서, 데이터베이스, 파일)를 주로 다루었으나 UML 2.0 버전으로 넘어오면서 개념적 내용을 다루는 독립적 다이어그램으로 변화하였다.

<img src="/_img/2022-08-23/component-diagram-sample.png">

컴포넌트는 기본적으로 아래 그림과 같은 형태를 띈다. 우측 상단에 작은 사각형 모음 표시를 볼 수 있는데, 이것이 컴포넌트를 나타내는 기호다.

<img src="/_img/2022-08-23/component-symbol.png">

컴포넌트 사이 인터페이스는 아래 그림에서처럼 표현된다. 둥글게 말아져있는 쪽이 인터페이스를 수신하는 쪽, 원이 연결되 쪽이 인터페이스를 전송하는 쪽이다.

<img src="/_img/2022-08-23/interface-symbol.png">

위 개념을 차용해 의존성을 표시할 수 있다. 컴포넌트 간에도 의존성이 존재할 수 있는데, 아래와 같은 방법으로 두 컴포넌트 사이 Dependency 를 표현할 수 있다. 

<img src="/_img/2022-08-23/dependency-symbol.png">

컴포넌트는 여러 개의 인터페이스 포인트를 가질 수 있다. 이 부분을 포트라고 하는데, 포트는 컴포넌트 한 면에 사각형을 붙여 표시할 수 있다. 아래와 같이 외부로 연결되는 부분을 표시할 때 사용된다.

<img src="/_img/2022-08-23/port-symbol.png">

### Composite Structure Diagram

Composite Structure Diagram 은 앞서 설명했던 Component Diagram 을 계층적으로 표현할 때 사용한다. 컴포넌트는 실행시간에 인스턴스화되는 하나의 개체라고 생각할 수 있는데, 이들을 다시 한 번 논리적 단위로 계층화 할 때 Composite Structure Diagram 이 이용되는 것이다.

<img src="/_img/2022-08-23/UML-composite-structure-diagram.png">

사용자는 Composite Structure Diagram 으로부터 시스템이 어떤 Obejct 를 가지고 있는지, 어떻게 상호작용하는지, 어떻게 동작하는지와 같은 전체적인 정보를 얻어낼 수 있다. 또한 현재 시스템 상태를 더 쉽게 이해할 수 있고, 상호작용하는 내부 구조를 더 세밀하게 관찰할 수 있는 기회를 제공한다.

### Deployment Diagram

앞서 작성한 Component Diagram 을 특정 PC, Server 의 Process 나 Thread 에 Assign 하여 Execution 하는 것은 Deployment Diagram 에 나타낸다. Deployment Diagram 은 아래와 같은 문장으로 설명된다.

> Deployment Diagrams depict a static view of the runtime configuration of hardware nodes and the software components running on those nodes.

즉, 작성한 Component Diagram 이 실제 어떤 Hardware Node 위에서 동작하는지를 표현하며, 아래 예시에서 확인할 수 있다. 이렇게 실제 배포 환경을 설명하는 다이어그램의 특성 때문에, UP 에서는 Construction Phase 나 Transition Phase 에 표현되는 것이 일반적이다.

<img src="/_img/2022-08-23/deploymentDiagram.png">

### Sequence Diagram

Sequence Diagram 은 오브젝트 상호작용(Collaboration of Object)을 시간의 순서에 따라 나타낸 다이어그램이다. Sequence Diagram 은 특정 기능이 동작할 때 참여하는 Object 들을 나열하고, 시간에 따라 각각이 어떻게 상호작용하는지를 보여준다.

<img src="/_img/2022-08-23/sequenceDiagramEnrollInSeminar.png">

Sequence Diagram 은 많은 정보를 내포하고 있으나 동작에 참여하는 Object 들이 많고, 과정이 길어지는 만큼 복잡도가 높아지는 단점도 있다. 아래 그림은 특정 Use Case 를 표현하는 Sequence Diagram 이다.

<img src="/_img/2022-08-23/sequenceDiagramBasicCourse.png">

### Communication Diagram

앞서 설명한 Sequence Diagram 의 복잡도 문제를 해결하기 위해 사용되는 다이어그램이 Collaboration Diagram 이라고도 불리는 Communication Diagram 이다. Communication Diagram 은 OO application 의 오브젝트들 사이의 Message Flow 를 기술하고, 클래스들 사이의 관계를 내포한다.

Communication Diagram 은 실제 동작 환경에서 오브젝트들이 어떻게 협업하는지를 넓은 관점에서 파악할 수 있게 돕고, 설계자로 하여금 복잡한 동작의 구현을 논리적으로 미리 구현해 볼 수 있도록 한다.

아래 그림은 세미나 상세 내용을 페이지에 표시하는 과정에서 각 오브젝트들 사이에 어떤 Message 들이 흘러가는지를 표현한 다이어그램 예시다.

<img src="/_img/2022-08-23/communicationDiagram.png">

이 때, 오브젝트끼리 주고 받는 메시지는 `1:getName():seminarName` 에서 볼 수 있듯이 `[sequenceNumber:] methodName(parameters) [: returnValue]` 형태의 Message notation 을 따른다.

이런 Communication Diagram 은 결론적으로 Sequence Diagram 과 비슷한 Expressive Power 를 가지며, 외부 사람이나 보고용으로 간단하게 활용하고자 할 때 사용된다.

### Timing Diagram

특정한 기능이 동작하기까지 어떤 순서로, 응답과 동작까지 걸리는 시간 등을 상세히 나타낸 다이어그램이다. Sequence Diagram 을 옆으로 눕히고 각 단계마다 상세한 시간을 기록한 형태와 비슷한데, ms 단위 처리가 필요한 Real-time 시스템의 상세 동작 요구사항을 정의할 때 사용된다.

<img src="/_img/2022-08-23/Website Timing.png">

### Activity Diagram

Activity Diagram 은 Business Process 를 설명할 때 주로 사용된다. 일종의 Flow-chart, Data Flow Diagram 이라고 여길 수 있다. 

<img src="/_img/2022-08-23/Ticket Vending Machine.png">

기본적으로 Activity Diagram 은 State Chart Diagram 은 일의 흐름을 나타낸다는 점에서 유사한 특징을 보여주지만, 명확한 차이점이 존재한다. Activity Diagram 은 State 에서 일을 하지만, State Diagram 은 발생한 Event 발생했을 때, State 가 변화하는 때에 일한다는 점이다.

### Interaction Overview Diagram

Interaction Overview Diagram 은 Activity Diagram 을 모아 놓은 것이다. Interaction Overview Diagram 의 한 노드 안에는 Sequence Diagram 과 Class Diagram 이 들어있어 특정 요구 동작을 설명한다.

일반적으로 시스템 전체가 어떻게 동작하는지 설명하거나, 시스템 테스트 시나리오를 작성하기 위한 자료로 참고한다. 

<img src="/_img/2022-08-23/800px-Uml-Iod-Diagram1.png">

### State (Statechart) Diagram

Statecharts formalism 을 따르는 UML의 다이어그램이다. 시스템이 유한한 상태를 가지는 Finite State Machine 일 때, 각각의 상태는 발생하는 이벤트에 따라 변화하게 된는데, 이를 State Diagram 으로 그리면 목적에 따라 Sequence Diagram, Class Diagram 을 대신하는 용도로 사용할 수 있다. 

<img src="/_img/2022-08-23/Bank ATM!State Model!Bank ATM!Bank ATM State Model_21.png">

## References

- [infocenter-archive.sybase.com](https://infocenter-archive.sybase.com/help/index.jsp?topic=/com.sybase.stf.powerdesigner.eclipse.docs_15.0.0/html/clug/clugp33.htm)
- [smartdraw.com](https://www.smartdraw.com/component-diagram/)
- [lucidchart.com](https://www.lucidchart.com/pages/uml-composite-structure-diagram)
- [agilemodeling.com](http://agilemodeling.com/artifacts/deploymentDiagram.htm)
- [docs.staruml.io](https://docs.staruml.io/working-with-uml-diagrams/timing-diagram)