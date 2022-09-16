---
title: "[SW Engineering] Class Diagram"
excerpt: "class diagram"
date: 2022-09-09
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

# Class Diagram

아래 그림은 Class Diagram 에 나오는 모든 Notation을 한 곳에 그린 예제다. 클래스 이름만 표기된 Coarse-grained 스타일부터 Attribute, Operation 모두 작성된 Fine-grained 스타일까지 모두 나타나있다. 뿐만 아니라 클래스 사이 다양한 Relationship 까지 확인할 수 있기 때문에 복습에 매우 용이하다. 지금부터는 각 항목들이 어떤 의미를 지니는지 확인해보자.

<img src="/_img/2022-09-09/common class diagram notation.png">

## UML Class Diagram

OOAD 에서는 UML Class Diagram 을 두 가지 종류로 사용한다. OOA 에서는 Entity 이름과 Attribute 정보만을 포함하는 Domain Model, OOD 에서는 정확한 Operation 을 포함해 Code Generation 수준의 내용까지 포함하는 Design Class Diagram (DCD) 이다. 공통점은 역시 둘 다 Class Diagram Notation 을 사용한다는 것이다.

### Object Diagram

[[SW Enginering] Object-Oriented Programming](https://wch18735.github.io/software%20engineering/SWEng_Object-Oriented/) 에서 Object 의 정의를 아래와 같이 설명했었다. 즉, 객체란 상태와 행위를 개체를 뜻한다고 할 수 있다. Object Diagram 이란 이러한 Object 이 특정한 시점(at a specific moment in time)에서 어떻게 존재하는지를 묘사한 일종의 모식도다.

> An object is an entity with a well-defined boundary and identity that encapsulates state and behavior

UML 에서는 Object 를 다이어그램으로 표현할 수 있다. 아래 그림은 Object 를 다이어그램으로 나타낸 예시다. 먼저 Object 을 표현하는 방법을 알아보자. UML 에서는 밑줄과 함께 소문자로 Object Name 을 기재해 해당 다이어그램이 Object 임을 나타낸다. 여기에 더해 Object 을 생성하기 위한 Class 를 표현하고 싶다면 콜론(:)과 대문자를 이용할 수 있다. 예를 들자면 <u>d1:Department</u> 는 **Department** 클래스 형태 인스턴스 **d1** 을 뜻한다.

<img src="/_img/2022-09-09/object diagram.png">

이름이 없는 Anonymous Object 은 두 가지 상황에서 사용된다. 먼저 현재 시점에서 Object 이름을 지칭할 수 없을 때에는 Object 이름을 지정하지 않는다. 예를 들어 아직 Sequence Diagram 이 구체화되지 않아 Object 이 정해지지 않았을 때에는 해당 다이어그램을 Anonymous Object 으로 남겨둔다. 두 번째로는 굳이 지칭할 필요가 없을 때인데, 일반적으로 사용되는 Object 일 때 역시 따로 이름을 기재하지 않는다.

### From Object to Class

Class 는 일련의 Object 들 중 비슷한 것들을 모아 구조화 한 것이라 정의할 수 있다. 그렇기 때문에 다이어그램으로 표현할 때, Class 와 Object 은 차이가 분명히 나타난다.

> A class is a construction plan for a set of similar objects of system

먼저 Class 는 밑줄과 소문자로 표현되는 이름이 존재하지 않는다. 소문자와 밑줄로 표현되는 것은 클래스의 인스턴스인 Object 이기 때문이다. 또한 Class 는 특정한 값을 가지지 않는다. Object Diagram 에는 특정 Attribute 에 할당되는 값이 기재되나 Class Diagram 에는 단순히 Attribute 의 자료형만을 나타낼 뿐 상세한 값을 지정하지는 않는다. 만약 Class Diagram 의 Attribute 에 값이 지정되어 있다면, 이는 Default 값을 뜻한다.

## Attribute Syntax 

Class Diagram 은 Attribute 과 Operation 을 가지고 있다. 먼저 Attribute Syntax 를 알아보자.

### Visibility

Class Diagram Visibility 를 표현하는 방법이 있다. 아래 Attribute, Operation 좌측에 기호가 붙어있는 것을 확인할 수 있을 것이다. 참고로 return type 에 붙은 Money 는 Value Type 으로 사용하는 클래스를 나타내는 것이다. 왜 Integer Type 이 아닌 사용자 지정 타입을 사용하는지는 Domain Driven Development 책에서 조금 더 상세히 설명되어있다.

<img src="/_img/2022-09-09/attribute visibility.png">

각각은 기호에 따라 아래와 같은 접근 수준을 나타낸다. 이 때, 아무것도 표시하지 않으면 Default Visibility 를 부여하게 되는데, Attribute 영역은 private, Operation 은 public 이 각각 default 로 설정되어 있다.

 - `+` public: everybody can access
 - `-` private: only the object itself
 - `#` protected: class itself and subclasses
 - `~` package: class that are in the same package

### Derived Attribute

Derived Attribute 은 내부 또는 외부 정보를 통해 계산되어 저장되는 속성을 뜻하며, 아래 그림에서와 같이 슬래시(/) 표시를 붙여 표기한다. 예를 들어, selectionValue 값은 화면에서 선택된 항목들만의 가격 합계를 표기해야 하는 계산식에 의해 값을 주입받는다.

<img src="/_img/2022-09-09/derived attribute.png">

### Type

Attribute 는 두 가지 종류로 나뉘게 된다. 하나는 단순 데이터 타입으로 Boolean, Integer, String 과 같은 Pre-defined 타입이나 Enumeration 등을 말한다. 반대로 사용자가 데이터 타입을 직접 정의할 수 있는데, 앞서 잠깐 설명했던 반환 값 형태 Money 가 그 예시가 될 수 있다. 이들은 Class Diagram 을 그릴 때, 우측또는 좌측에 간단히 정의해 놓고 사용할 수 있다. 

### Multiplicity

Class 에는 한 개 데이터 뿐만 아니라 같은 타입의 데이터를 여러 개 담고있는 변수를 만들 수 있다. 이를 Attribute 에서 표현해주기 위해 사용하는 문법이 Multiplicity 이다. `Notation: [min..max]` 처럼 사용할 수 있으며, 배열이나 리스트로의 구현은 구현부에서 수행하게 된다.

### Properties

Attribute 들은 특징을 가질 수 있으며, Attribute 우측에 중괄호(`{ }`)로 닫힌 속성들을 표기하여 나타내게 된다. 기존에 미리 정해진 성질들은 아래와 같으며, 한 가지 뿐만 아니라 여러 Property 들이 기재될 수 있다. 가령 `address: String[1..*] {unique, ordered}` 은 주소 속성에 문자열로 구성된 주소들이 최소 1개 이상이 존재하며, 각각은 유일하고 정렬된다는 것을 뜻한다.

- Pre-defined properties
  - {readOnly}: value can not be changed
  - {unique}: no duplicates premitted
  - {non-nuique}: duplicates permitted
  - {ordered}: fixed order of the values
  - {unordered}: no fixed order of the values

이들을 이용하면 특정한 자료구조를 표현할 수도 있다. 가령 예시로 사용했던 `{unique, ordered}` 는 Ordered Set 을 나타낸다. 비슷한 예시들은 아래에 정리하였다.

- Attribute specification
  - Set: {unordered, unique}
  - Multi-Set: {unordered, non-unique}
  - Ordered-Set: {ordered, unique}
  - List: {ordered, non-unique}

## Operation Syntax

Operation 이란 기본적으로 클래스가 제공하는 기능을 표현한다. 이런 기능을 표현하는 Operation Syntax 를 알아볼 것인데, 알아둬야 할 한 가지 사실은 Class Diagram 에서 보여주는 Operation 은 특정한 Object Oriented Programming Language 에 종속되지 않는다는 것이다(Java, C++, Python 등).

### Parameters

아래 그림을 보면 Operation 에 나타나는 Parameter 들 옆에 `in` 또는 `out` 이 붙어있는 것을 확인할 수 있다. `in` 은 Input Parameter 로 Operation 이 동작할 때 내부에서만 사용하는 매개변수를 뜻한다. 반면 `out` 은 내부에서 사용한 매개변수가 외부에도 연결됨을 뜻한다. 이는 매개변수에 대한 **Call by Value** 와 **Call by Reference** 개념으로 설명할 수 있다.

Operation 의 오른쪽에는 반환 값이 적힌다는 것을 알 수 있다. 만약 아무것도 반환되지 않으면 void 값을 적어준다. 그 아래 getPersonNumber 아래 밑줄은 Class Variable 이나 Class Operation 을 나타내기 위해 사용되었다. 설명을 쉽게 하자면 클래스를 만들 때 static 으로 표현되는 Variable 또는 Operation 이 예시가 될 수 있다.

<img src="/_img/2022-09-09/operation syntax - parameters01.png">

## Operations and Methods

Operation 은 `visibliity name (parameter-list) : return type {property-string}` 포맷을 가진다. Attribute 와 달리 이름 왼쪽에 아무것도 표시되어 있지 않으면 기본적인 Visibility 는 Public 값을 나타낸다. 이러한 Operation 을 Method 와 착각하는 경우가 종종 발생한다. 

Operation 은 이름, 매개변수, 반환형, 예외 조건 등으로 표현되는 하나의 정의를 뜻한다. 이러한 Operation 은 아직 Body 가 없으나 Method 는 이러한 앞서 정의한 내용을 C++ 또는 Java 를 통해 구현한 것을 말한다. 그렇기 때문에 같은 Operation 에 대해 여러 Method 가 나타날 수도 있음을 알고 있어야 한다.

## Note Notation

UML 에서는 주석과 같이 Note Symbol 을 이용해 작성자가 추가적으로 기입하려는 내용을 표시할 수 있다. Note Symbol 은 사각형 우측 상단이 접힌 것으로, 아래는 Bank Account 클래스 다이어그램에 owner, balance 조건을 표시한 예시다. 

<img src="/_img/2022-09-09/note notation.png">

## Types of Class Relationship

Class Diagram Relationship 과 관련된 자료를 찾다가 아주 잘 정리된 글이 있어서 참고차 첨부하였다. [https://www.nextree.co.kr/p6753/](https://www.nextree.co.kr/p6753/) 에서 조금 더 상세하 내용을 읽어보면 좋을 것 같다.

<img src="/_img/2022-09-09/class diagram relationships.png">

### Generalization

Generalization 은 부모 클래스와 자식 클래스의 상속 관계를 표현할 때 사용된다. 일반적으로 상위 부모 클래스의 Attribute 목록을 공유하며, 공통적으로 동작하는 Operation 은 재정의하지 않아 재사용성을 높이며, 필요한 경우 Overriding 하여 사용하는 것을 말한다.

또한, Generalization 시에는 단순 Attribute 또는 Operation 뿐만 아니라 모든 Association 들을 함께 공유하게 된다. 그렇기 때문에 Generalization 을 가장 강력한 Relationship 이라고 한다. 표현은 간단하지만 구현 수준은 매우 복잡해질 수 있다는 것을 알아야 한다.

### Realization

반면 Realization 은 일반화와 달리 Interface 에 명시된 Specification 을 구체화하는 것을 뜻한다. 실제로 인터페이스 구현의 경우 점선과 빈 삼각형으로 구성된 Realization UML 표기를 따르는 것을 확인할 수 있다.

<img src="/_img/2022-09-09/09-uml-realization-example.png">

### Dependency

Dependency 는 한 가지 클래스가 다른 한 클래스를 참조하는 것을 뜻한다. 의존성이란 단어는 소프트웨어 공학뿐만 아니라 실제 개발에서도 많이 사용되는 단어로 익숙하게 느껴질 것이다. 가장 대표적으로는 웹 백엔드 개발에 사용되는 외부 의존성 주입이 떠오른다.

참조의 형태는 대상 클래스를 어떻게 사용하는지에 따라 달라지게 되는데, 아래 그림에서 처럼 `<<create>>`, `<<local>>`, `<<parameter>>` 등으로 그 용도를 나타낼 수 있다. 아럐 예시는 User 클래스 안에서 Schedule 클래스를 생성하고 내부 변수로 사용하며, 매개변수로서 사용하고 있음을 표현하고 있다.

<img src="/_img/2022-09-09/class diagram dependency.png">

### Association / Direct Association

Association 은 기본적을 클래스 사이의 연관 관계를 나타낸다. 한 클래스에서 다른 클래스의 Reference 를 가지고 있다면, 이는 연관 관계가 존재한다고 얘기할 수 있다. 이러한 연관 관계를 잘 이해하고 정확히 기술하려면 Navigability 라 불리는 방향성을 숙지하고 있어야 한다.

방향성이란 단순하게 소유 관계를 명시하는 것이다. 단순 실선으로 표시된 Association 으로 연결된 두 클래스는 서로가 서로를 참조할 수 있다. 이와 달리 한 쪽의 소유 관계만을 가지는 Direct Association 은 화살표를 통해 소유자와 피소유자를 구분짓게 한다. 화살표 시작점에 위치한 소유자는 화살표의 대상을 참조할 수 있으나 역은 불가능한 것이다.

<img src="/_img/2022-09-09/navigability.png">

이러한 Association 은 Class 로 존재할 수 있다. JPA 에서 Many to Many 관게를 표현할 때, 중간에 Association Table 을 생성하는 것으로 이해하고 있다. 두 클래스가 서로를 참조할 수 있는데 각각이 Tenary Reference 일 때와 같은 경우 유용하게 사용할 수 있다. 관계 자체를 클래스로 나타내고, 각각은 관계를 참조하도록 만들어 놓는 것이다.

### Aggregation / Composition

Aggregation 은 Shared Aggregation 이라고도 불리며 Association 관계를 조금 더 특수하게 나타낸 것으로 전체와 부분(Whole and Part) 관게를 나타낸다고 한다. 그러나 이 개념 외에 명확한 정의를 제공하지 않아 여러 프로그래머나 분석가, 설계자들의 혼란을 야기했다고 한다.

Composition 역시 전체와 부분이라는 집합적인 관계를 표현하는데 사용되며 다이아몬드 내부가 채워져 있다는 차이가 있다. 또한 Composition 은 Aggregation 보다는 조금 더 강한 관계를 표현하기 위해 사용된다. 가령 Composition 에서는 전체(Whole) 인스턴스가 부분(Part) 인스턴스의 전체 수명을 책임지며, 각 부분에 해당하는 인스턴스들은 공유될 수 없다. 

아래는 자동차와 타이어의 관계를 Aggregation 과 Composition 으로 표현했을 때, 옳은 것과 옳지 않은 것을 표현한 예다. 보이는 것처럼 존재할 수 있는지 여부를 판단하는 것이 참ㆍ거짓을 판단하는 중요한 요인이 된다.

<img src="/_img/2022-09-09/shared aggregation and composition.png">

이 외에도 [블로그](https://www.nextree.co.kr/p6753/)에서 설명하는 예시처럼 Object Copy 의 두 가지 경우인 Shallow Copy 와 Deep Copy 에 대해서도 생각해 볼 수 있다. 복사가 수행될 때, Object 가 가지고 있는 모든 Reference 가 단순히 복사되는 것이 아니라 타입과 내용이 동일한 것으로 새로 생성되는 것이 Composition 이라고 할 수 있다. 이처럼 생성과 복사, 삭제와 관련된 생애주기는 전체 인스턴스를 따라야 하기 때문에 부가적인 구현이 필요할 수 있다.

## Singleton Classes

 Singleton Class 는 오직 한 개 인스턴스만 존재하는 클래스를 말한다. UML 다이어그램에서는 우측 상단에 1을 적어줌으로써 해당 클래스가 Singleton 클래스라는 것을 나타낼 수 있다.

## Active Classes

Active Class 는 추후 Main Program 과 독립적으로 프로세스 또는 쓰레드를 할당받아 인스턴스 생성되는 것을 뜻한다. 아래 그림과 같이 클래스 양쪽에 실선을 그어 표현한다. 여기서 Clock 클래스가 Runnable Interface 를 Realization 하고 있다는 것까지 알아두면 좋다.

<img src="/_img/2022-09-09/active class01.png">

## Conclusions

There is no Silver Bullet! 클래스 다이어그램은 자동으로 만들어지지 않는다. 좋은 클래스 다이어그램을 만들기 위해서는 클래스 다이어그램 작성 경험과 심도 깊은 사용자 요구 사항 분석을 통해야 한다. 힘들어도 계속적으로 UML Tool 을 사용하고, 클래스 다이어그램을 따라 구현해보며 다양한 관점에서 경험을 쌓는 것이 중요하겠다.