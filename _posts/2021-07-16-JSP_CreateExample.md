---
title: "[JSP] 간단한 Servlet 프로그램 생성 예제"
excerpt: "Servlet practical program example"
date: 2021-07-16
layout: single
classes: wide
category:
    - jsp
tag:
    - java
    - jsp
    - servlet
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

## Servlet 프로그램 예시

서버는 `GET:/notice/list` 나 `GET:/notice/reg` 와 같은 요청에 응답을 보낸다. 이때, 브라우저의 요청에 따라 동적으로 만들어지는 웹의 형태가 달라진다. 특정한 요청이 발생할 때 어떤 방식으로 정보를 처리하는지를 알아보자.

모든 서블릿 클래스들은 *WAS* 에 의해 로드되며, 약속된 *인터페이스* 나 *추상 클래스* 를 상속해 기능을 정의한다. 

인터페이스와 추상 클래스. 공부하고 또 공부해도 헷갈리는 개념이다. 오늘도 어김없이 복습 겸 간단하게 두 개념을 정리하자.

||인터페이스|추상 클래스|
|---|---|---|
|추상메서드 선언|있음|있음|
|선언방법|interface InterfaceName|abstract class ClassName|
|다중 상속|가능|불가능|

곧 바로 서블릿 코드 예제를 보자. 아래는 *UserDefinedServlet* 서블릿을 예제 코드다.

```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.*;

public class UserDefinedServlet extends HttpServlet {
    public void service(
        /* User describe functionality */
    );
}
```

이렇게 작성된 서블릿은 각각 특정 기능을 수행한다. 어떻게 동작하는가는 `service()`에 작성된다.

## Servlet 프로그램 실습 환경 설정

이번 글에서는 <span style="color: #2255FF; font-weight: bold">Visual Studio Code</span>를 사용해 실습할 예정이다. vscode 위 톰캣 환경 구성을 위해 [여기](https://nevertrustbrutus.tistory.com/245)를 참고했다.

**Tomcat** 을 설치한 뒤 vscode에서 실행할 때 초록불이 안 들어오는 현상이 있을텐데 **StackOverflow**에서는 Setting.json 파일에서 `"java.home"`에 해당하는 변수를 주석 처리하면 해결된다고 한다. 아래는 간단한 과정이다.

1. ctrl + shift + p
2. 검색창에 settings.json 입력
3. settings.json 파일 내부의 java.home 을 주석 처리

## Servlet 프로그램 실습

vscode 안의 terminal에 다음을 입력.

`javac -cp C:\Users\wch18\Desktop\WCH\Aparches\apache-tomcat-9.0.48\lib\servlet-api.jar UserDefinedServlet.java`

이때, servlet-api.jar 가 있는 경로를 `-cp` 옵션을 이용해 컴파일 시 추가한다. 이를 진행하면 클래스 파일이 생성되며, 이렇게 생성된 바이트 코드가 바로 **서블릿 클래스** 이다.

그렇다면 두 가지 궁금증이 생긴다.

- 이 클래스들은 어디에 저장되어야 할까?
- 이 클래스들은 어떻게 호출되어 실행될까?

 이들은 `webapps/ROOT/WEB-INF/classes/` 에 보관된다. 참고로 클래스들을 포함하는 패키지가 있다면, **classes** 내부에 패키지와 동일한 구조를 형성해 주어야 WAS가 이를 찾을 때 에러가 발생하지 않는다. 그리고 이러한 클래스들이 보관된 WEB-INF 폴더는 절대 외부에서 직접 접근할 수 없다.

해당 폴더에 클래스를 옮겨 놓았다. 그렇다면 클라이언트의 요청에 어떻게 호출될까에 대한 의문이 든다. WAS는 브라우저의 요청에 대해 **미리 매핑된** 서블릿을 동작시켜 보낸다. 이러한 매핑 관계는 **WEB-INF 폴더 내부 web.xml 파일**에 기록되어 관리한다.

아래는 `http://localhost/hello` 로 요청이 들어오게 되면 서블릿이 실행되는 코드이다. 아래와 같은 매핑을 통해 URL 작성자는 모든 폴더 구조를 알지 않아도 특정 동작을 요청할 수 있다.

```xml
<servlet>
    <servlet-name>s_name</servlet-name>
    <servlet-class>UserDefinedServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>s_name</servlet-name>
    <url-pattern>/hello</url-pattern>
</servlet-mapping>
```

위의 코드를 기존 톰캣이 설치된 폴더의 WEB-INF 내부 web.xml 에 다음과 같이 입력한다.

![web xml](/_img/2021-07-12/web_xml.jpg){: .align-center}

그리고 `localhost:8080/hello` 를 브라우저에 입력해주면 강의에서는 흰 화면이 나타나게 된다. 그러나 실습을 진행하며 아래와 같은 에러가 발생했다.

![web xml](/_img/2021-07-12/exception_error.jpg){: .align-center}

## Trouble Shooting

이를 자세히 들여다보면 어떤 부분에서 에러가 발생했는지 자세히 적혀있다.

```
UserDefinedServlet has been compiled by a more recent version of the Java Runtime (class file version 59.0), this version of the Java Runtime only recognizes class file versions up to 56.0
```

버전 충돌이 발생하는 것이라 설명하고있다. 현재 버전은 **56.0** 까지 인식하지 못하나 컴파일은 **59.0** 으로 되어 에러가 발생하는 것이라 이해했다. 실제로 JDK는 12를 사용하고 있으나 OS 상의 자바는 15로 설정되어 있었다.

기존 Java를 모두 지우고, jdk-12.0.2를 받아 환경변수 세팅까지 마치고 나니 에러가 없어졌다. 

```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.*;

public class UserDefinedServlet extends HttpServlet {
    public void service(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException
    {
        OutputStream os = response.getOutputStream();
        PrintStream out = new PrintStream(os, true);
        out.println("Hello Servlet!!");
    }
}
```

끝으로 UserDefinedServlet.java 내용을 위와 같이 바꾸고 새로 컴파일해 기존 것을 대체한 결과 아래와 같이 해당 내용이 성공적으로 웹 화면에 출력되었다.

![success](/_img/2021-07-12/success.jpg){: .align-center}

그러면 매번 이 작업을 진행해야 하는가? 에 대한 의문이 생긴다. 

IDE를 사용하면 모든 과정을 보다 빠르게 진행할 수 있는데, 나는 *Eclipse* 보다 *vscode* 내에서 모든 작업을 해보고 싶다. vscode 내에서도 complile을 통해 `.class` 파일을 지정된 위치에 생성하는 방법을 알아보는 것이 다음 과제가 될 것 같다.