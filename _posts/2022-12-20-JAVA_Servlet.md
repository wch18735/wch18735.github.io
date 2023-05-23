---
title: "[Java] Servlet"
excerpt: "servlet"
date: 2022-12-20
category:
    - java
tag:
    - http
    - side project
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
classes: wide
comments: true
---

# 서블릿 프로그래밍

서블릿은 동적 웹 페이지를 생성하는 근본 기술이다.

## CGI (Common Gateway Interface)

CGI(Common Gateway Interface)란 웹 서버와 애플리케이션 사이에 데이터를 주고받는 **규약**이다. 이름에서 알 수 있듯이, CGI는 어디까지나 인터페이스이기 때문에 특정 플랫폼에 의존하지 않고, 웹 서버 등으로부터 외부 프로그램을 호출하는 조합 자체를 가리킨다. 그렇기 때문에 CGI 프로그램은 컴파일 방식(C, C++, Java 등)과 인터프리터 방식(PHP, Python 등) 모두 구현될 수 있다.

처음 웹이 등장했을 때는 정적인 데이터인 HTML, XML, 이미지만을 전달하는 것으로도 충분했으나, 시대가 변하고 요구사항을 만족하기 위해 사용자의 입력에 따라 동적인 결과를 보여주는 페이지들이 필요하게 되었다. 이런 역할을 수행 할 수 있도록 도와준 기술이 바로 CGI이다. 쉽게 설명하면 CGI는 웹 서버에서 동적인 페이지를 보여 주기 위해 임의의 프로그램을 실행하는 기술 중 하나이다. 

아주 오래된 사이트에서는 /cgi-bin/으로 시작하는 주소를 볼 수 있고, 실제로 2000년대 전후 대한민국에 개인 홈페이지 만들기 붐이 불었을 때, cgi 기술이 개인 홈페이지 게시판 프로그램에 가장 널리 활용되었다고 한다. /cgi-bin/은 무로 게시판 프로그램을 돌릴 때 쓰던 디렉토리 지정 방식이었다고 하는데, 가장 큰 문제는 요청이 하나 들어 올 때마다 프로세스가 하나씩 실행되었다는 것이다. 이는 코드를 실행할 때마다 매번 코드를 해석하는 스크립트 언어에 치명적이었다고 한다.

## Servlet

서블릿(Servlet)은 Server + Applet 합성어이다. 여기서 Applet은 플러그인의 하나로서 아주 작은 응용 프로그램을 말한다. 즉, Servlet은 서버에서 돌아가는 아주 작은 응용 프로그램이라고 해석해 볼 수 있다. 여기서 앞에 Java가 붙으면 Java Servlet이 되는데, 이 Java Servlet이 바로 Tomcat이 이해할 수 있는, 순수 Java 코드로만 이루어진 서버에서 돌아가는 **아주 작은 응용 프로그램**이다. 이전 CGI는 프로세스 단위로 돌아 시스템에 부하가 컸다면, Servlet은 스레드 단위로 실행되어 부하를 줄일 수 있다.

프로그램은 명령을 수행하고 결과를 반환한다. 위에서 Java Servlet도 아주 작은 응용 프로그램이라고 말했다. 그로부터 Servlet도 프로그램과 마찬가지로 서버로 전달된 요청을 수행하고, 그 결과를 반환해주는 역할을 한다는 것을 알 수 있다. 실재로 Servlet은 자바에서 동적인 웹 페이지를 구현하기 위한 표준으로서 사용되고 있다.

## ServletContainer

앞서 웹 계산기 요청을 처리하기 위한 Custom Server를 만들기 위해 몇 가지 작업을 수행했었다. 웹 서버 포트를 설정하고, 소캣을 만들어 관리했으며, 특정한 요청이 전달되면 Thread Pool에서 Thread를 생성하고, 내부에 로직을 심어 요청에 따른 동작을 수행하도록 했다. 한 두 가지 기능은 괜찮지만, 시스템이 커질 수록 포트 관리, 소켓 관리, Thread 관리 등 많은 부분들을 수동으로 처리해줘야 하는 것에 부담을 느낄 수밖에 없을 것이다.

이럴 때, 서블릿 컨테이너는 서블릿의 생성부터 소멸까지 생애주기를 모두 관리하는 역할을 한다. 즉, 서블릿 컨테이너는 웹 서버와 소켓을 만들고 통신하는 과정을 대신 처리해주어 개발자가 비즈니스 로직에만 집중 할 수 있도록 해준다.

한 가지 주의할 점으로 서블릿 컨테이너는 서블릿 객체를 인스턴스 하나만 생성하여 공유하는 방식인 싱글톤으로 관리한다는 것이다. 잘 알겠지만, 싱글톤 객체는 Thread와 함께 사용될 때는 상태를 유지(stateful)하게 설계하면 안 된다. 왜냐하면 공유되는 영역에 Race condition이 형성되기 때문이다. 그렇기 때문에 Thread safety를 위해 서블릿 객체 내부에는 static 변수를 설계하지 않는 것이 가장 바람직하다.

### Singleton 클래스에서 Thread Unsafety 확인

Singleton 클래스에서 상태를 유지하는 경우를 확인해보려고 한다. 그러기 위해 Counter 객체에 count 변수를 0으로 초기화한다.

<img src="/_img/2022-12-20/counter.png">

이를 아래와 같이 Race condition을 유발하도록 세 개의 스레드에서 공유하여 사용하도록 바꿔보자. 그러면 스레드 실행 순서를 특정할 수 없기 때문에 예상했던 것과 다른 출력이 나타나는 것을 확인할 수 있다.

<img src="/_img/2022-12-20/thread result.png">

## 계산기 서블릿 만들기

### Servlet 인터페이스 활용하기

servlet은 Javax.servlet.Servlet 인터페이스로 제공된다. 이를 구현하는 abstract class인 javax.servlet.GenericServlet이 있고, 다시 한 번 이를 구현하는 javax.servlet.http.HttpServlet이 있다. 이전 개발자들은 이 추상 클래스를 상속하여 본인만의 서블릿을 작성하였다.

<img src="/_img/2022-12-20/servlet relation.png">

서블릿 컨테이너는 서블릿에 정의된 메소드만을 호출한다. 다형성에 의해 인터페이스에 정의된 init(), service(), destroy()를 호출하면, 사용자가 구현한 실제 메소드가 실행된다. 예를 들어, 아래와 같이 Servlet 인터페이스를 구현하는 CalculatorServlet을 만들어 볼 수 있다.

<img src="/_img/2022-12-20/CalculatorServlet.png">

여기에 아래와 같이 Logger를 추가하고, HTTP GET 메소드를 요청해보면 service 메소드가 요청마다 호출되는 것을 볼 수 있다. 한 가지 유심히 봐야 할 것은 init() 메소드는 생성될 때 단 한 번만 호출된다는 것이다. 끝으로, destroy()는 확인하지 않았지만 할당된 자원을 해제하는 등의 역할을 수행한다.

<img src="/_img/2022-12-20/servlet execution.png">

### GenericServlet 추상 클래스 활용하기

위에서 Servlet 인터페이스를 통해 구현한 기능들을 GenericServlet 추상 클래스를 사용해 동일하게 구현해보려고 한다. 방법은 간단하게, implements를 extends로 바꿔주고 GenericServlet을 상속해 구현한다. GenericServlet은 init(), destroy(), getServletConfig(), getServletInfo() 메소드들이 모두 작성되어 있기 때문에 사용자는 service() 메소드만을 구현하면 된다.

### HttpServlet 추상 클래스 활용하기

마지막으로 HttpServlet을 사용할 수 있다. GenericServlet 추상 클래스는 여전히 service() 메소드를 호출하지만, 그 안에서 METHOD에 따라 다른 동작을 수행하기 위해 사용자가 직접 분기 처리를 수행해야 한다. 이러한 불편함을 해결하기 위해 HttpServlet은 service() 메소드 내부에 METHOD에 따라 분기 처리가 되어있고, doGet(), doPost(), doDelete() 등의 추상 메소드가 정의되어 있다.

<img src="/_img/2022-12-20/HttpServlet if.png">