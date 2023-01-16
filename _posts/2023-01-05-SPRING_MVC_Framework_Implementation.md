---
title: "[Spring] Spring MVC Framework 만들기"
excerpt: "spring mvc framework self implementation"
date: 2023-01-05
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - spring mvc
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Spring MVC Framework 만들기

간단한 Spring MVC Framework 프로토타입을 만드는 과정을 통해 DispatcherServlet 동작 흐름과 원리를 파악한다.

<img src="/_img/2023-01-05/mvc framework flowchart.png">

## METHOD 고려하기: HandlerKey 활용

HandlerMapping에서 적절한 Handler(=Controller)를 파악할 때, 가장 간단하게 생각해 볼 수 있는 구현 방법은 문자열을 키로 하고 값을 핸들러로 가지는 Map을 선언하는 것이었다. 그렇기 때문에 HandlerMapping으로 Handler를 요청할 때 전달하는 값은 request.getRequestURI()로 충분하다고 생각할 수 있다.

그러나 아래와 같이 전달되는 URI만으로 Handler를 찾으려고 하면 문제가 발생한다. 바로 서로 다른 HTTP METHOD로 전달되는 요청을 구분할 수 없다는 것이다. **Controller 한 개에 합치고 내부에서 분기해 사용하면 안 되나요?** 라는 질문을 할 수 있다. 

HTTP METHOD는 GET, HEAD, POST, PUT, DELETE, CONNECT, OPTIONS, TRACE, PATCH 까지 9개가 있다. 이들 모두를 if-else 분기처리해서 한 개 코드 안에서 관리한다는 발상은 유지보수 측면에서 큰 걸림돌이 될 수 있다.

<img src="/_img/2023-01-05/controller with same uri.png">

이 때, METHOD를 고려하기 위해 HandlerKey 객체를 키 값으로 사용한다. 이 HandlerKey를 키 값으로 사용하면 DispatcherServlet에서 HandlerMapping으로 적절한 Handler를 요청하는 코드는 아래 그림처럼 같은 URI에 대해 서로 다른 METHOD를 등록할 수 있는 코드로 바뀐다.

<img src="/_img/2023-01-05/add handler key.png">

HandlerKey는 RequestMethod, URI Path를 속성으로 가지는 객체다. DispatcherServlet에서는 HttpServletRequest 객체 내부 속성 값인 METHOD, URI를 받아와 객체를 전달해 사용한다. 

아래 코드에서는 Enum 타입인 RequestMethod에서 값을 추출하기 위해 valueOf를 사용하는 것을 보여준다. request.getMethod()가 반환하는 문자열과 Enum 내부 값을 비교해 일치하는 값을 반환해 사용한다.

<img src="/_img/2023-01-05/dispatcher servlet with handler key.png">

## Handler Adapter (forward vs redirect)

HandlerKey를 통해 서로 다른 Handler를 구분할 수 있게 되었다. 그러나, Redirect 요청이 전달될 때에는 Redirect와 Forward를 구분하지 못 하고 비정상적인 동작을 수행하게 되는 경우가 있다. 이를 해결하기 위해, Handler Adapter를 거치도록 한다.

HandlerAdapters에 현재 HandlerKey 값에 대응되는 handler를 받아와 전달한다. 그러면 해당 handler 타입을 지원할 수 있는 가장 적절한 HandlerAdapter를 골라낸다. 

그리고 골라낸 handlerAdapter의 handle 메소드에 함께 전달되는 handler가 요청과 응답을 받아 ModelAndView를 반환하게 된다. 이 과정을 담은 것이 아래 코드이다.

<img src="/_img/2023-01-05/dispatcher servlet service final.png">

끝으로, viewResolver에 받아 온 modelAndView로부터 뷰 이름을 전달받도록 코드를 작성하면 DispatcherServlet과 ViewName의 독립성을 유지하며 동작시킬 수 있다. 가령 viewResolvers에는 JspViewResolver가 있는데, 해당 Resolver는 JspView 또는 RedirectView를 로직에 따라 반환한다.

해당 코드에서는 for 문을 사용해 모든 viewResolver들에게 동작을 위임하지만, 이것은 잘 못 된 로직인 것 같아 추후에 수정이 필요할 것 같다는 생각이 든다.