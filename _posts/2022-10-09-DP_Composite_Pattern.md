---
title: "[Design Pattern] Composite Pattern (복합체 패턴)"
excerpt: "composite pattern"
date: 2022-10-09
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

# Composite Pattern

Composite Pattern은 클라이언트가 복합 개체나 단일 객체를 모두 동일하게 취급되어야 하거나 부분과 전체가 모두 트리 구조를 형성되어야 할 때 사용된다. 이러한 문제를 해결하는데 사용되는 Composite Pattern은 부분(Leaf)과 전체(Composite)에서 사용할 Component 인터페이스를 통합하고, Composite 객체들은 해당 요청을 Component 인터페이스를 직접 구현한 자식 객체(Leaf)로 전달함으로써 구축할 수 있다.

- Purpose
  - Facilitates the creation of object hierarchies where each object can be treated independently or as a set of nested objects through the same interface
- Use When
  - Hierarchical representations of objects are needed
  - Objects and compositions of objects should be treated uniformly

## Structure: Class and Object diagram

아래는 Composite Pattern의 클래스 다이어그램이다. 클라이언트는 컴포넌트의 operation() 인터페이스를 사용한다. 이 때, 클라이언트가 가리키는 Component가 Leaf라면 원하는 동작을 수행하도록 구현한다. 반대로 Composite이라면 operation()에 child 관계로 연결된 Component의 operation()을 사용한다.

<img src="/_img/2022-10-09/composite pattern.png">

이를 Object Diagram으로 나타내면 아래와 같다. 실제로 Composite은 Component 타입을 담는 컬렉션을 가지고 있어 Leaf와 Composite을 모두 담을 수 있다. 그렇기 때문에 composite1은 composite2와 leaf1, leaf2를 모두 담고 있다.

<img src="/_img/2022-10-09/composite pattern object diagram.png">

## Iterator for Tree structure

위와 같인 Composite Pattern은 필연적으로 Tree 구조를 생성하게 된다. 그렇다면 클라이언트가 Composite을 순회하며 정보를 조회하고자 할 때, 해당 구조를 참고하여 순회 방법을 직접 구현해야 할까? 

정답은 그렇지 않다. 이렇게 단순하지 않은 컬렉션 구조를 가진 정보 군집에 대하여 클라이언트는 Composite가 어떤 구조로 이뤄져있는지 정보 은닉한 상태로 순회 방법을 변경하는 것이 가능하다. 바로 Iterator를 이용하는 것이다.

```java
public class Menu extends MenuComponent {
	Iterator iterator = null;

// other code here doesn't change
	public Iterator createIterator() {
		if (iterator == null)
			iterator = new CompositeIterator(menuComponents.iterator());
		return iterator;
	}
}

public class MenuItem extends MenuComponent {
// other code here doesn't change
	public Iterator createIterator() {
		return new NullIterator();
	}
}
```

이 때, Composite과 달리 Leaf는 반환할 Iterator가 존재하지 않는다. 그치만 아래와 같이 Null 값을 반환해 존재하지 않는 것을 표현하는 **Null Object Pattern**을 사용해 Composite과 Leaf를 동시에 같은 방법으로 다루도록 코드를 작성할 수 있다.

```java
// Composite Iterator
public class CompositeIterator extends Iterator {
	Stack stack = new Stack();

	public CompositeIterator(Iterator iterator) {
		stack.push(iterator);
	}

	public Object next() {
	if (hasNext()) {
		Iterator iterator = (Iterator)stack.peek();
		MenuComponent component = (MenuComponent)iterator.next();
		if (component instance of Menu)
			stack.push(component.createIterator());
		return component;
		}
	else return null;
	}

	public boolean hasNext() {
		if (stack.empty())
			return false;
		Iterator iterator = (Iterator) stack.peek();
		if (!iterator.hasNext()) {
			stack.pop();
			return hasNext();
		} else
			return true;
	}

	public void remove() {
		throw new UnsupportedOperationException();
	}
}

// Leaf Iterator
public class NullIterator implements Iterator {
	public Object next() {
		return null;
	}

	public boolean hasNext() {
		return false;
	}

	public void remove() {
		throw new UnsupportedOperationException();
	}
}

```

## Transparency vs Safety

Composite가 사용하는 add(), remove(), getChild()를 Component에 위치시킨다면 Transparency가 높아진다. 모든 컴포넌트는 똑같이 다뤄지기 때문이다. 그러나 이는 Leaf가 add(), remove(), getChild() 메소드를 호출할 수 있게 만들기 때문에 안정성을 해치게 된다.

반대로 이를 Composite에만 위치시킨다면 안정성을 높일 수 있다. 그러나 앞선 예제와 같이 Leaf와 Composite이 서로 다른 Interface를가지고 있기 때문에 Transparency를 잃게 된다.

