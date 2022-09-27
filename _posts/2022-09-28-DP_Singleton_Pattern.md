---
title: "[Design Pattern] Singleton Pattern (싱글톤 패턴)"
excerpt: "singleton pattern"
date: 2022-09-28
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

# Singleton Pattern

- Purpose
  - Ensures that only one instance of a class is allowed within a system.
- Use When
  - Exactly one instance of a class is required.
  - Controlled access to a single object is necessary.
- One of a Kind Objects
  - One and only one instance is sometimes good for management; Window Manager, Printer Spooler, Thread Pool Manager, Caches, Logging, Factory
  - We want to instantiate them only once
- Why is it difficult after all?
  - Global variables and Multi-threading issue
  - Coding idiom

## Skeleton of Singleton

<img src="/_img/2022-09-28/singleton skeleton.png">

싱글톤 패턴 설계 포인트는 아래와 같다.

1. 생성자(Constructor)를 private으로 만든다.
`private Singleton() {}`를 통해 외부에서 생성자를 사용하면 에러가 발생하도록 만든다.

2. getInstance() 메소드를 제공한다.
`public static Singleton getInstance()`를 통해 생성하고자 하는 인스턴스를 반환하도록 한다. 이 때, static 키워드를 사용해 해당 싱글톤 클래스 인스턴스를 만들지 않고서도 사용 할 수 있도록 한다.

3. 두 번 생성되지 않도록 한다.
`private static Singleton uniqueInstance`에 할당되는 인스턴스가 존재하는 경우 재생성하지 않도록 한다. 만약 해당 값이 null 값이라면 클래스 내부에서 단 한 번 **new** 생성자를 통해 만들어 할당하도록 한다. 이를 구현한 코드가 getInstance() 메소드 내부 조건문에 해당한다.

### Example: Applying Singleton

```java
public class MemoryManager {
    private boolean empty;
    private boolean turned;
    private static MemoryManager uniqueInsatnce;

    private MemoryManager() {
        empty = true;
        turned = false;
    }

    public static MemoryManager getInstance() {
        if(uniqueInstance == null){
            uniqueInstance = new MemoryManager();
        }

        return uniqueInstance;
    }

    // ...
}
```

### Class Diagram

<img src="/_img/2022-09-28/singleton class diagram.png">

## Multi-Thread Problem

### Solution Option 1

**synchronized** 키워드를 사용해 한 개 Thread에 대한 일관성을 유지한다. 그러나 이러한 방법은 getInstance() 호출 횟수가 많아지는 경우, 예를 들어 스레드 개수가 증가함에 따라 성능 이슈가 발생하게 된다.

```java
public class Singleton {
	private static Singleton uniqueInstance;

    // other useful instance variables
	private Singleton() {
	}

	public static synchronized Singleton getInstance() {
		if (uniqueInstance == null) {
			uniqueInstance = new Singleton();
		}
		return uniqueInstance;
	}
    // other useful methods 
}
```

### Solution Option 2

2번 방법은 시작과 동시에 객체를 전달하는 방법이다. 그러나 이러한 방법은 생성하려는 객체가 로딩 시간이 오래 걸리는 무거운 오브젝트인 경우, 초기에 성능 이슈가 발생하게 된다.

```java
public class Singleton {
	private static Singleton uniqueInstance = new Singleton();

    // other useful instance variables
	private Singleton() {
	}

	public static Singleton getInstance() {
		return uniqueInstance;
	}
    // other useful methods 
}
```

그렇기 때문에 getInstance() 호출 횟수가 늘어남에 따르는 성능 저하를 방지하며, 동시에 진입에 따른 성능 저하가 발생하지 않도록 아래와 같은 형태로 나타낼 수 있다. 그러나 아래와 같은 경우 아주 극적이지만 다수의 Thread가 조건문을 동시에 통과하면 중복으로 인스턴스가 생성되는 경우가 발생할 수 있다.

```java
public class Singleton {
	private static Singleton uniqueInstance = null;

    // other useful instance variables
	private Singleton() {
	}

	public static Singleton getInstance() {
		if (uniqueInstance == null) {
			synchronized (Singleton.class) {
				uniqueInstance = new Singleton();
			}
		}
		return uniqueInstance;
	}
    // other useful methods 
}
```

이를 막기 위해, Lock 내부에 중복으로 인스턴스가 발생하는 것을 막도록 검사하는 조건문을 추가해 인스턴스가 생성되었는지 확인한다. 다수의 Thread가 동시에 `synchronized(Singleton.class)`를 통과해도 최초에 통과하는 Thread 하나만 싱글톤을 생성하도록 제어하는 것이다.

```java
public class Singleton {
	private static Singleton uniqueInstance = null;

    // other useful instance variables
	private Singleton() {
	}

	public static Singleton getInstance() {
		if (uniqueInstance == null) {
			synchronized (Singleton.class) {
				if (uniqueInstance == null)
					uniqueInstance = new Singleton();
			}
		}
		return uniqueInstance;
	}
    // other useful methods 
}
```

### Solution Option 3

그러나 앞선 상황에서 JVM이 관리하는 Thread들의 Synchronization 동작 방식에 의해 생성되지 않은 상태로 반환된 객체를 사용해 Runtime Error를 발생시키는 이슈가 있었다.

이를 해결하기 위해 volatile 키워드를 추가해 다수의 Thread에서 공유되는 특정 변수가 동기화되는 것을 보장하도록 작성한다. 참고로, volatile은 Java version에 영향을 받으며 1.5 이후가 되어서야 유효한 효과를 보장한다고 한다.

```java
public class Singleton {
	private volatile static Singleton uniqueInstance = null;

    // other useful instance variables
	private Singleton() {
	}

	public static Singleton getInstance() {
		if (uniqueInstance == null) {
			synchronized (Singleton.class) {
				if (uniqueInstance == null)
					uniqueInstance = new Singleton();
			}
		}
		return uniqueInstance;
	}
    // other useful methods 
}
```
