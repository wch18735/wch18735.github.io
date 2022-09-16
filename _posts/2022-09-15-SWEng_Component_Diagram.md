---
title: "[SW Engineering] Component Diagram"
excerpt: "uml component diagram"
date: 2022-09-15
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

# Component Diagram

UML Components는 관리 가능하고, 재사용 가능하며, 대체 가능한 소프트웨어의 단위를 뜻한다. 기본적인 UML을 익히고 디자인 패턴을 연습하고나면 그 다음 과정으로 앞서 말한 Component들을 구성하고, 배치하는 Architecture 설계 단계를 배우게 된다. 

이러한 Architecture를 보는 관점(View)과 관련해 Kruchten's 4 + 1 view model 에서는 Use Case View, Logical View, Process View, Physical View, Development View 까지 다섯 가지 관점을 제안한다. 이 때, Development View에 Deployment Diagram과 Component Diagram이 속하게 된다.

## UML Components

UML에서 Component는 독립적인 개발의 단위로 취급 가능하다. 예를 들어 특정 Component는 특정 개발팀에 맡길 수 있다. 이를 바꿔 말하면 Component Diagram은 소프트웨어 개발 초기보다는 후미에 많이 등장하게 된다.

아래 그림은 Component Diagram을 구성하는 Component, Provided Interface, Required Interface, Port, Connectors 정보가 모두 표기된 Component Diagram Notaion 예제다.

<img src="/_img/2022-09-15/component-diagram-overview.png">

### Component

먼저 Component Diagram에서 컴포넌트를 표현하는 개념을 표현하는 그림은 아래와 같다. 컴포넌트는 **\<\<component\>\>** 라는 키워드를 사용해 표시하며, UML 2.0에서 처음 소개되었다.

<img src="/_img/2022-09-15/component.png">

추가적으로 아래와 같이 우측 상단에 사각형 모양을 넣어 컴포넌트를 표현하기도 하는데, 이는 UML 1.4 에서 사용되는 아이콘이다. 만약 해당 아이콘이 사용된다면 앞서 컴포넌트를 표현하기 위한 식별자는 생략한다.

<img src="/_img/2022-09-15/component-icon.png">

또한, UML 1.x 버전과의 하위 호완성을 위해 아래와 같은 형태로 나타내는 경우도 있다고 한다.

<img src="/_img/2022-09-15/component-obsolete.png">

**External View** 또는 **Black Box** 컴포넌트 개념도는 컴포넌트의 속성들을 표현할 때 사용한다. 컴포넌트들은 외부와의 연결을 수행하는 Interface 정보를 비롯해 개별적인 동작이나 속성을 지닐 수 있다.

<img src="/_img/2022-09-15/component-external-view-compartments.png">

### Provided / Required Interface

컴포넌트끼리는 인터페이스를 통해 정보를 주고 받을 수 있는데, 정보를 전송하는 부분을 Provided Interface라고 하며 반대로 정보를 수신하는 부분을 Required Interface라고 한다.

아래는 Provided Interface 개념도 예제로 날씨 서비스 컴포넌트에서 날씨 예보를 전송하는 것을 나타낸다. Interface는 실체화(Realization) 후 사용되는 것이 원칙임은 모두가 알고있는 사실이다. 해당 인터페이스 역시 컴포넌트 자체나 컴포넌트가 포함한 내부의 다른 컴포넌트로부터 실체화(Realization)되어 사용된다. 직접 연결되는 경우도 있으며 그림에 나타나지 않았지만 Port를 통해 연결되는 경우도 있다.

<img src="/_img/2022-09-15/component-provided-interface.png">

반면 아래는 Required Interface 개념을 예제로 나타낸 것으로 표현하려는 핵심 내용은 의존성(Dependency)이다. 아래에서는 어떤 인터페이스를 사용하는지를 나타내고 있는데, Lollipop Symbol을 사용하거나 점선과 빈 화살표 머리를 사용해 실체화하는 방식을 설명한다.

<img src="/_img/2022-09-15/component-required-interface.png">

### Component Realization

Component Realization은 컴포넌트가 의존성을 가지는 것을 표현한다. 예를 들어 아래와 같이 Service 컴포넌트를 UserSevlet과 UserDAO에서 구현해 사용하는 경우가 대표적이다.

<img src="/_img/2022-09-15/component-realization.png">

이를 전체적인 정보를 담아 표현하기 위해 아래와 같이 나타낼 수 있다. artifact 부분에는 실제 어떤 jar 파일에 이 컴포넌트가 구현되는지를 표기한다.

<img src="/_img/2022-09-15/component-realization-compartments.png">

만약 실체화 단계에서 계층적 표기가 필요하다면 컴포넌트 내부에 아래와 같이 나타내는 것도 가능하다. 아래 그림은 UserServlet이 UserDAO를 포함하는 관계를 보여주고 있다.

<img src="/_img/2022-09-15/component-realization-nested.png">

### Delegation / Assembly Connector

Delegation Connector는 외부로 연결되는 특정한 연결점이다. 아래와 같이 외부로 나가거나 들어오는 연결 지점에 이름을 표현해 역할을 명시해준다. 이는 한 개의 포트에서 여러 개 인터페이스가 맞물려 모두 표현하기 불가할 때 주로 사용한다.

<img src="/_img/2022-09-15/delegation-connector-part.png">

반면 Assembly Connector는 다른 컴포넌트의 파트나 포트로 연결되는 부분을 표현한다. 아래 그림은 그 예시이며, 같이 맞물리는 지점에는 **ball-and-socket** 또는 **lollipop** 스타일이라 불리는 형태로 접점을 나타내는 것을 확인 할 수 있다.

<img src="/_img/2022-09-15/assembly-connector-multiple-ports.png">