---
title: "[Design Pattern] Command Pattern (명령 패턴)"
excerpt: "command pattern"
date: 2023-04-03
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

# Command Pattern

Command Pattern은 객체 지향 디자인 패턴 중 하나로, 실행될 기능을 객체화하여 클라이언트와 리시버(수신자) 사이 의존성을 줄이는 패턴이다. 클라이언트는 리시버 객체의 구체적인 구현과 결합하지 않고, 명령 객체를 통해 실행될 기능을 저장할 수 있다.

Command Pattern은 아래와 같은 요소들로 구성되어 있다. 설명만으로는 어려울 수 있으나 예시 코드와 함께 읽어보면 쉽게 이해할 수 있을 것이다.

먼저 명령 인터페이스를 선언한다.

```java
// Command 인터페이스
public interface Command {
    void execute();
}
```

위에서 선언한 명령 인터페이스는 구체화 될 수 있다. 아래는 `ConcreteCommand1`, `ConcreteCommand2`로 인터페이스를 구현한 코드다.

```java
// ConcreteCommand1 클래스
public class ConcreteCommand1 implements Command {
    private Receiver receiver;
    
    public ConcreteCommand1(Receiver receiver) {
        this.receiver = receiver;
    }
    
    public void execute() {
        receiver.action1();
    }
}

// ConcreteCommand2 클래스
public class ConcreteCommand2 implements Command {
    private Receiver receiver;
    
    public ConcreteCommand2(Receiver receiver) {
        this.receiver = receiver;
    }
    
    public void execute() {
        receiver.action2();
    }
}
```

각 클래스는 Receiver를 인자로 가지고 있다. 즉, 명령 객체는 해당 명령을 수행하는 수행 주체의 정보를 가지고 있다고 생각할 수 있다. 수행 주체는 다양한 명령을 가지고 있지만, **나에게 해당하는 명령**만을 수행하도록 작성되어 있다.

명령을 호출하는 객체는 특정한 명령 객체를 설정한 뒤 그 객체에 `executeCommand()`를 통해 실행 명령을 내린다. 명령 호출 객체는 명령 종류가 얼마나 많든 `명령 설정 → 실행` 과정만 거치면 되기 때문에 실제 실행 주체로부터 연관성을 떨어트릴 수 있다.

```java
// Invoker 클래스
public class Invoker {
    private Command command;
    
    public void setCommand(Command command) {
        this.command = command;
    }
    
    public void executeCommand() {
        command.execute();
    }
}
```

명령을 수신하는 객체, 이를테면 리모컨과 같은 객체는 내부에 각 명령들에 대해 어떤 동작을 할 지 미리 정해져있다.

```java
// Receiver 클래스
public class Receiver {
    public void action1() {
        System.out.println("action1 실행");
    }
    
    public void action2() {
        System.out.println("action2 실행");
    }
}
```

아래는 앞서 살펴본 코드를 실행해보는 Client 코드다.

```java
// Client 클래스
public class Client {
    public static void main(String[] args) {
        // 리시버 객체 생성
        Receiver receiver = new Receiver();
        
        // 명령 객체 생성
        Command command1 = new ConcreteCommand1(receiver);
        Command command2 = new ConcreteCommand2(receiver);
        
        // Invoker 객체 생성
        Invoker invoker = new Invoker();
        
        // 명령 객체 설정
        invoker.setCommand(command1);
        invoker.executeCommand(); // action1 실행
        
        invoker.setCommand(command2);
        invoker.executeCommand(); // action2 실행
    }
}
```

## Undo / Redo

명령을 객체로 나타낼 수 있다는 개념을 조금만 생각해보면, 명령을 저장했다가 다시 복구하는 것도 실행할 수 있다는 개념도 가능하다. 이는 `특정 작업에 대한 반대 작업`을 기록해 놓거나, `특정 작업 후 상태를 저장`하여 기록할 수 있다.

가령 아래와 같이 덧셈을 하는 명령에 대해, 반대 급부로 뺄셈을 하는 명령을 나타냈다고 가정하자.

```java
// AddCommand 클래스
public class AddCommand implements Command {
    private Receiver receiver;
    private int value;
    
    public AddCommand(Receiver receiver, int value) {
        this.receiver = receiver;
        this.value = value;
    }
    
    public void execute() {
        receiver.addValue(value);
    }
    
    public void undo() {
        receiver.subValue(value);
    }
}
```

아래는 실질적으로 명령을 수행하는 `Invoker` 클래스에서 `Stack` 자료구조를 이용해 이전에 사용했던 명령들 목록을 저장하는 방법을 보여준다.

```java
// Invoker 클래스
public class Invoker {
    private Stack<Command> undoStack = new Stack<>();
    private Stack<Command> redoStack = new Stack<>();
    
    public void executeCommand(Command command) {
        command.execute();
        undoStack.push(command);
        redoStack.clear();
    }
    
    public void undoCommand() {
        if (!undoStack.empty()) {
            Command command = undoStack.pop();
            command.undo();
            redoStack.push(command);
        }
    }
    
    public void redoCommand() {
        if (!redoStack.empty()) {
            Command command = redoStack.pop();
            command.execute();
            undoStack.push(command);
        }
    }
}
```

이렇게 템플릿 코드가 작성되었으면, 명령을 수행하는 수신자에 구체적인 동작 방법을 구현해준다. 아래는 덧셈과 뺄셈에 대한 구현이다.

```java
// Receiver 클래스
public class Receiver {
    private int value = 0;
    
    public void addValue(int value) {
        this.value += value;
        System.out.println("현재 값: " + this.value);
    }
    
    public void subValue(int value) {
        this.value -= value;
        System.out.println("현재 값: " + this.value);
    }
}
```

실행하는 `Invoker`는 명령이 어떻게 동작하는지와는 관계 없이, `undo()` 명령어를 수행하면 이전과 같은 형태로 돌아가게 된다.

```java
// Invoker 클래스
public class Invoker {
    private Stack<Command> undoStack = new Stack<>();
    private Stack<Command> redoStack = new Stack<>();
    
    public void executeCommand(Command command) {
        command.execute();
        undoStack.push(command);
        redoStack.clear();
    }
    
    public void undoCommand() {
        if (!undoStack.empty()) {
            Command command = undoStack.pop();
            command.undo();
            redoStack.push(command);
        }
    }
    
    public void redoCommand() {
        if (!redoStack.empty()) {
            Command command = redoStack.pop();
            command.execute();
            undoStack.push(command);
        }
    }
}
```

위 코드를 참고해 작성한 테스트 코드는 아래와 같다.

```java
// Client 클래스
public class Client {
    public static void main(String[] args) {
        // 리시버 객체 생성
        Receiver receiver = new Receiver();
        
        // Invoker 객체 생성
        Invoker invoker = new Invoker();
        
        // AddCommand 객체 생성
        Command command1 = new AddCommand(receiver, 10);
        Command command2 = new AddCommand(receiver, 5);
        
        // 명령 객체 실행
        invoker.executeCommand(command1); // 현재 값: 10
        invoker.executeCommand(command2); // 현재 값: 15
        
        // undo 실행
        invoker.undoCommand(); // 현재 값: 10
        
        // redo 실행
        invoker.redoCommand(); // 현재 값: 15
    }
}
```