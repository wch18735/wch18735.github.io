---
title: "[Java] Thread를 활용한 응답"
excerpt: "mini server with thread"
date: 2022-12-21
category:
    - java
tag:
    - http
    - thread
    - side project
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
classes: wide
comments: true
---

# Thread를 활용한 응답

Custom Web Application Server 내부에 `logger.info("[CustomWebApplicationServer] {}", Thread.currentThread().getName());` 코드를 추가하고 다시 HTTP 테스트를 진행해보면 아래와 같은 결과를 확인 할 수 있다. 바로 main 쓰레드가 응답을 담당하고 있는 것이다.

<img src="/_img/2022-12-20/thread main.png">

Main Thread는 해당 응답을 처리하기 위해 다양한 작업을 수행하는 것을 확인했다. 가령 문자열을 파싱하거나 데이터베이스 Access를 수행하여 응답이 오기까지 Idle 상태가 될 수도 있다. 이런 종류의 요청이 만약 100개가 동시에 들어온다면, Main Thread는 100반의 작업을 순차적으로 처리하는데 생각보다 오랜 시간이 걸리게 되고, 마지막 요청에 대해서는 요구사항을 만족하지 못 할 수도 있다.

이런 일을 방지하고자, 우리는 Thread를 사용한다. Java에서는 Runnable 인터페이스를 구현하는 클래스를 Thread 인자로 전달하여 초기화하고, 해당 Runnable 구현 객체를 실행한다. 간단한 예로 main 스레드가 아닌 신규 스레드를 생성하여 요청에 대한 응답을 처리하는 방식으로 코드를 바꿔 볼 수 있다.

<img src="/_img/2022-12-20/thread by runnable.png">

위 코드를 실행하면 아래와 같이 Thread-0 이름으로 실행되는 Thread를 확인 할 수 있다. 이렇게 작성하면 여러 요청이 동시에 들어와도 각 요청에 맞게 Thread를 생성해 전달해주기만 하면 되기 때문에 응답 시간 지연 이슈를 방지 할 수 있다.

<img src="/_img/2022-12-20/thread zero.png">

## Thread Pool

앞서 설명한 방법대로 코드를 작성하면 Application 서버 코드 자체도 더욱 간결하게 표현할 수 있다.

<img src="/_img/2022-12-20/logic replace.png">

그러나 위 방법도 이슈가 발생한다. 만약 100개가 아니라 10,000개 요청이 동시에 전달된다고 가정하자. 구현한대로 동작한다면 Main 쓰레드는 10,000개의 쓰레드를 만들어 제공한다. 이것이 문제가 될 수 있다. 쓰레드를 만드는 것도 많은 Context Switch에 따른 오버헤드를 야기하며, 심한 경우 OOM(Out Of Memory)를 띄울 가능성도 배제할 수 없다. 이같은 현상을 막기 위해 우리는 Thread Pool을 만들어 한정된 자원만으로 많은 요청들을 처리할 수 있게 한다.

최종적인 코드는 아래와 같이 작성할 수 있으며, HTTP 테스트를 진행하면 pool 번호와 thread 번호를 동시에 확인할 수 있다.

<img src="/_img/2022-12-20/thread pool.png">

## Thread 에 대한 재미있는 사실

메인 스레드(Main Thread)는 언제 종료될까?

Java에서 `main` 메서드가 종료되면 프로그램이 끝난다고 생각할 수 있지만, **항상 그렇지는 않다.**  
이유는 **데몬(daemon) 스레드** 때문이다.

### 데몬 스레드란?

- 메인 스레드가 종료되더라도 **데몬 스레드**가 남아 있으면 프로그램은 종료되지 않는다.
- **백그라운드 작업**(ex. 가비지 컬렉터, 로그 처리 등)에 사용된다.
- `setDaemon(true)` 로 설정 가능.

예제 코드는 아래와 같다.

```java
public class Main {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            while (true) {
                System.out.println("I'm still running...");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        thread.setDaemon(false); // true로 설정하면 메인 종료 시 함께 종료됨
        thread.start();

        System.out.println("Main thread is ending...");
    }
}
```