---
title: "[SW Engineering] Use Case Diagram"
excerpt: "use case diagram"
date: 2022-09-05
layout: single
classes: wide
category:
    - software engineering
tag:
    - software engineering
author: 1FeS
comments: true
header:
    teaser: "/_img/thumb-nail/software-engineering.png"
---

# Use Case

Use Case Diagram 은 사용자가 시스템을 사용하는 시나리오인 Use Case 를 정리해서 그려놓은 다이어그램이다. 선물 상자보다 그 안에 들어있는 선물이 더욱 중요한 것처럼 Use Case 가 Use Case Diagram 안에 들어가 있는 것으로, Use Case 가 Use Case Diagram 보다 훨씬 중요하다고 말할 수 있다. 이런 Use Case 의 보다 정확한 정의는 다음과 같다.

> Use cases are test stories of some actors using a system to meet goals. It is a mechanism to capture, identify and analyze requirements.

UP 에서는 Use Case 가 Functional Requirement 를 대체할만큼 중요한 역할을 수행한다. 이러한 Use Case 는 글로 작성되며, 작성된 상세함의 정도에 따라 Brief → Casual → Fully Dressed 단계로 나누어진다. 예를들어 *마우스 왼쪽 버튼을 누르면 마우스 포인터가 기리키는 컴포넌트를 누른다* 라는 문장은 마우스 클릭이라는 Use Case 를 Brief Format 으로 나타낸 것이다.

처음 Use Case 를 작성할 때는 Brief Format 으로 두 세 줄 정도로 기재한다. 이후, 특정 Iteration 에 작성했던 Use Case 를 구현하기로 했다면 조금 더 구체화된 형태인 Casual Format 으로 작성해 요구동작을 상세히 나타낸다. 예를 들어, Casual Format 부터 Main Success Scenario, Alternative Scenario, Exceptional Scenario 가 나타나는데 단순히 어떻게 동작했으면 좋겠다는 것을 넘어 예외상황이나 정상동작하지 않을 때의 행위까지 구체화하게 된다. 마지막 Fully Dressed Format 에서는 주로 시스템이 실행되기 전 Preconditions 를 정의하고, Extentions 라는 메인 시나리오 외 다른 확장된 시나리오들을 상세히 정의한다.

## Use Case Diagram

Use Case Diagram 은 Use Case, Actor, System 들 사이의 관계를 표현한 그림이다. 시스템이 어떤 동작들을 수행하는지 일목요연하게 확인할 수 있어 System context diagram 이라고도 불린다. 그러나 이는 시스템이 작고, Use Case Diagram 하나에 모든 Use Case 들을 담을 수 있는 경우에 한정된다. 만약 시스템이 복잡해진다면 여러 장의 Use Case Diagram 을 다양한 측면에서 시스템이 어떻게 사용되는지 기술해야 한다.

복잡한 시스템은 다양한 Actor 를 가진다. 여기서 Actor 란 시스템을 사용하는 주체를 말하는데, 기본적으로는 사용자가 되겠지만 컴퓨터 시스템이나 특정 단체가 되기도 한다. 이런 Actor 들은 종류에 따라 시스템을 사용해 목적을 달성하는 Primary Actor, 시스템에 특정 기능을 제공하는 Supporting Actor, 시스템과 연동되지만 밀접한 관계는 아닌 Offstage Actor 등으로 나뉜다.

### Example: Use Case Templates

Use Case 템플릿을 검색해보면 이미 다양한 양식이 존재하는 것을 확인할 수 있다. 추후 프로젝트 시작 단계에서는 여러 템플릿을 다운받아 핵심적으로 포함되어야 하는 항목들을 확인하고, 본인이 진행하는 프로젝트에 필요한 항목은 추가, 불필요한 항목은 제거해 Custom Use Case Templates 을 만들면 조금 더 매끄러운 프로젝트 관리가 이뤄질 수 있다.

<img src="/_img/2022-09-06/use case templates.png">

### Example: Fully Dressed Use Case

아래는 Fully Dressed Use Case 예시를 뒤져보다 UML 강의를 들었다면 누구나 들어봤을 Process Sale in a POS System 의 UC1: Process Sale 예제를 찾았다.

Main Success Scenario (Basic Flow) 에 대한 Extensions (Alternative Flows) 가 매우 상세하게 정의된 것이 가장 먼저 눈에 들어온다. UP 에서는 실제로 Funcion Requirement Specification 을 Use Case 로 대체하는데, 모든 시나리오가 아래 예시만큼 자세히 작성된다고 생각하면 충분히 가능한 얘기라고 생각한다.

<img src="/_img/2022-09-06/uc1-01.png">
<img src="/_img/2022-09-06/uc1-02.png">
<img src="/_img/2022-09-06/uc1-03.png">

## Guidelines

Use Case 를 작성하기 위한 몇 가지 가이드 라인을 살펴보자.

### Guideline: Write in an Essential Style

Use Case 를 작성할 때는 항상 Essential Contents 만을 포함하도록 유의해야 한다. 여기서 Essential Contents 란 UI free, Technology free, Method free 한 내용들을 말하는데, 그 반대는 세세한 내용이 모두 포함된 Concrete Contents 가 있다.

가령 Essential Style 로 관리자 인증 기능을 작성한다면 아래와 같이 나타낼 수 있다.

```bash
1. Administrator identities self
2. System authenticates identity
3. ...
```

이것을 Concrete Style 로 작성한다는 것은 아래와 같다. 아래 예시에서는 사용자 인증 과정에 필요한 정보를 ID와 PW로 처음부터 고정하고 있는데, 이는 구현 시 충분히 달라질 수 있는 부분이다. Use Case 에서는 달성하려는 목표 기능에 집중하고, 나머지 설계나 구현과 관련된 부분은 뒷단으로 위임하는 태도가 매우 중요하다.

```bash
1. Administrator enters ID and PW in dialog box
2. System authenticates Administrator
3. System displays default window with ...
4. ...
```

## Guideline: Write Black-Box Use Case

Use Case 를 작성할 때에는 위에서 언급했던 것처럼 설계와 구현 영역은 후미로 위임하는 것이 중요하다. 같은 맥락으로, Use Case 를 작성할 때는 내부 동작을 Black-Box 처럼 다루는 것이 중요하다.

예를들어, System 에서 발생하는 정보는 Database 에 저장하고, 데이터 삽입 시에는 INSERT 쿼리를 사용한다는 세부적인 지시사항은 Use Case 에서 다뤄지기에는 부적합한 내용이다. 같은 기능이라도 구현 방법은 무수히 달라질 수 있기 때문에 기술 중심적은 내용은 제외하고 기능에 충실한 Black-Box Style Use Case 를 작성하도록 해야 한다.