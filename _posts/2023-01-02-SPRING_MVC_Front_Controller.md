---
title: "[Spring] Front Controller Pattern"
excerpt: "front controller pattern"
date: 2023-01-02
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - spring mvc
    - front controller pattern
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Front Controller Pattern

프런트 컨트롤러 패턴은 모든 요청을 단인 Handler(처리기)에서 처리하도록 하는 패턴으로 중앙 집중식 요청 처리 매커니즘을 가진다. 프런트 컨트롤러 패턴은 일반적인 웹 어플리케이션 패턴으로 앞단의 단일 서블릿이 다른 실제 프로세싱 컴포넌트에 책임을 위임하는 형태를 띈다. Spring Web MVC 프레임워크에서는 DispatcherServlet이 Front Controller 역할을 수행한다.

<img src="/_img/2023-01-02/dispatcher servlet flow diagram.png">

브라우저로부터 발생한 사용자 요청은 Front Controller Servlet이라고 불리는 DispatcherServlet을 만난다. DispatcherServlet의 역할은 전달받은 요청을 적절히 수행할 수 있는 서블릿에게 전달하는 나눠주는 것이다. 이렇게 적절한 서블릿을 확인하기 위해 DispatcherServlet은 HandlerMapping에게 요청 URI와 Method를 전달하고, HandlerMapping은 해당 요청을 처리할 수 있는 컨트롤러를 반환해준다.

전달된 컨트롤러에 요청이 흘러가면, 컨트롤러는 본인이 해당 요청을 처리하거나 서비스 오브젝트로 해당 요청을 위임하여 요청사항을 수행하게 된다. 때로는 결과를 조회하는 경우도 있고, 데이터를 단순 삽입하거나 수정하는 작업만을 수행해 반환해 줄 결과는 없이 완료 신호만 전달하는 경우도 있다. 어떤 경우든 반환되는 것이 있기 마련인데, 이를 Model이라 부른다. 

이러한 Model은 Raw Information으로, 사용자들에게 직접 전달되면 해석에 어려움을 겪게 된다. 정보는 사용자 친화적인 형태, 이를테면 HTML과 같은 형태로 전달되어야 하므로 명령을 수행한 컨트롤러는 마지막 작업으로 Model 데이터를 어떤 View에게 전달해 패키징 할 것인지를 알려준다.

이 때, 컨트롤러가 특정 뷰에 직접적으로 연결되지 않도록 한 가지 트릭을 더 추가한다. 바로 HandlerMapping처럼 Map 자료구조를 이용하는 ViewResolver를 두어, DispatcherServlet이 모든 뷰 이름을 기억하지 않고, 특정 JSP를 직접 식별하지 않도록 만드는 것이다. 대신 결과를 생성할 실제 뷰를 검색하는 데 사용되는 논리적 이름을 ViewResolver에 전달하고, 이름에 매핑된 반환받은 View를 취한다.

끝으로 View는 Model 데이터를 사용해 결과를 랜더링해 응답 객체에 담아 클라이언트에 전달하게 된다. 여기까지가 위 그림을 간결하게 설명한 것으로, Spring MVC 흐름에서 DispatcherServlet의 동작을 중심으로 요청과 응답이 처리되는 큰 흐름을 모두 설명한 것이다.

## Forward vs Redirect

DispatcherServlet이 요청을 다루는 방법은 Forward 방식과 Redirect 방식이 있다. Forward는 서블릿에서 클라이언트(웹 브라우저)를 거치지 않고 바로 다른 서블릿이나 JSP에게 요청한는 방식이다. Forward 방식은 서버 새부에서 일어나는 요청이기 때문에 HttpServletRequest, HttpServletResponse 객체가 새롭게 생성되지 않으며 이를 공유한다. 사용 방법은 `RequestDispatcher dispatcher = request.getRequestDispatcher("Forward 대상 서블릿 또는 JSP")`를 통해 RequestDispatcher를 생성해 `dispatcher.forward(request, response)`와 같이 요청과 응답을 그대로 전달해준다.

Redirect는 서블릿이 클라이언트(웹 브라우저)를 다시 거쳐 다른 서블릿이나 JSP에게 요청하도록 만드는 방식이다. Redirect는 클라이언트로부터 새로운 요청을 받기 때문에 새로운 HttpServletRequest, HttpServletResponse 객체가 생성된다. 사용 방법은 HttpServletResponse 객체의 sendRedirect() 메소드를 사용한다.

## Front Controller Implementation Example

먼저 아래와 같이 톰캣 서버를 실행하는 코드를 작성해준다. 조금 더 자세한 설명은 [[WEB Calculator] HTTP와 Socket - Custom Server 구현](https://wch18735.github.io/web%20calculator/WEBCALC_CustomServer/) 부분을 참고하자.

<img src="/_img/2023-01-02/custom server.png">

그 다음 org.example.mvc 패키지 아래에 DispatcherServlet 클래스를 생성하고 init() 메소드와 service() 메소드를 작성해준다. HttpServlet을 상속하기 때문에 생성 시 가장 먼저 init() 메소드가 호출되고, 서블릿 호출 시 service() 메소드가 호출된다.

아래 코드는 간단하게 작성되어 있지만 위의 그림을 통해 설명하는 DispatcherServlet 흐름도를 코드로 확인해 볼 수 있다. 먼저 DispatcherServlet이 요청을 받으면 requestMappingHandlerMapping 객체에 **요청 URI**를 전달하여 이에 맞는 Controller(==Handler)를 찾아온다.

그 다음 handler.handleRequest(request, response)를 통해 요청과 응답을 전달해 처리 과정을 위임하는 것을 확인 할 수 있다. 사실 이 반환 값으로 바로 viewName과 함께 Model이 전해져야 하고, viewName을 ViewResolver에 전달해 View를 받아오는 작업을 거쳐야 한다.

받아 온 viewName을 통해 전달 할 JSP 파일을 특정하고, 그 곳으로 requestDispatcher.forward(request, response) 수행해 요청과 응답을 forward 한다. 마지막은 해당 JSP가 반환되며 동작이 마무리 된다.

<img src="/_img/2023-01-02/dispatcher servlet simple code.png">

## References

- [StackOverflow: What is DispatcherServlet in Spring?](https://stackoverflow.com/questions/2769467/what-is-dispatcher-servlet-in-spring)
- [패스트캠퍼스: 10개 프로젝트로 끝내는 웹 개발](https://fastcampus.co.kr/dev_online_befinal)