---
title: "[JSP] Web 저장소 활용법 예시 (Application, Session, Cookie)"
excerpt: "remaining result or data as global variables"
date: 2021-08-20
layout: single
classes: wide
category:
    - jsp
tag:
    - java
    - jsp
    - servlet
    - application
    - session
    - cookie
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 웹 상태 유지 (application, session, cookie)

계산기 웹 어플리케이션을 제작한다고 가정하겠습니다. 사용자는 아래 과정을 반복할 것입니다.

- 숫자를 입력
- 연산을 선택
- 결과 확인

이때, 여러 번의 POST가 발생하게 되며 서버는 이전 결과 값을 기억하고 있어야 현재 입력으로 주어진 **연산** 에 대한 결과 값을 출력할 수 있습니다. 

그러나 **Servlet**은 서버 프로그램의 조각들로 이전 값을 기억하는 기능이 없습니다. 즉,
**전역변수**처럼 서블릿 이외 공간에 값을 저장해 사용해야 합니다. 이를 가능하게 해주는 것이 아래 5가지 방법을 확인해보겠습니다.

- **application 객체**
- **session 객체**
- **cookie**
- **hidden input**
- **querystring**

이 중 Servelet 에서 사용할 수 있는 상태 저장소는 아래와 같이 구분됩니다.

- **Server-side**
	- Application 저장소
	- Session 저장소
- **Client-side**
	- Cookie 저장소

이를 숙지하고 아래 정리한 내용을 읽어보면 좋을 것 같습니다.

## 예제 웹 페이지

![web example](/_img/2021-09-02/web_example.jpg){: .align-center}

먼저 실습을 위한 예제 HTML 코드 입니다. 간단한 입력창과 덧셈, 뺄셈, 등호 버튼을 보여주는 페이지 입니다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
 <form action="calc" method="post">
		<div>
			<label>input: </label>
			<input type="text" name="v" />
		</div>
		
		<div>
			<input type="submit" name="operator" value="+" />
			<input type="submit" name="operator" value="-" />
			<input type="submit" name="operator" value="=" />
		</div>
		<div>
			result: 0
		</div>
	</form>
</body>
</html>
```

## Application 객체로 상태 값 저장 (ServletContext)

서블릿들은 한 번 실행되고 난 뒤 사라집니다. 이런 서블릿들이 결과를 공유하거나, 이전에 사용했던 정보에 따라 동작을 달리하고 싶을 때 **Servlet Context**를 사용합니다.

**Context**라는 것은 사전적으로 문맥을 뜻합니다. 전체적인 문단의 흐름, 전체적인 정보를 뜻합니다. 현재 상태를 뜻하는 것으로도 쓰이며 CPU가 프로세스를 교체할 때 발생하는 **Context Switching**의 **Context**가 비슷한 의미로 사용됩니다.

Application 객체는 `import javax.servlet.ServletContext;` 를 통해 선언할 수 있습니다. **ServletContext**는 **Map Collection** 이라고 생각하면 사용이 편할 것입니다.

Application 객체에 값을 저장하는 방법과 불러오는 방법은 아래와 같습니다.

- **Set:** `application.setAttribute(key, value)`
- **Get:** `application.getAttrubute(key)`

필요에 따라 값을 불러온 뒤 자료형 **Wrapping**으로 문법적 오류를 피해야 하는 경우가 발생할 수 있습니다. 아래 코드는 위의 예제 HTML에 적용할 수 있는 서블릿 코드입니다.

```java
package com.jsp_example.web;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/calc2")
public class Calc2 extends HttpServlet{
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		PrintWriter out = response.getWriter();
		
		// Application 저장소
		// ServletContext 는 Map Collection 이라고 생각!
		ServletContext application =  request.getServletContext();
					
		String v_ = request.getParameter("v");
		String op = request.getParameter("operator");
		
		int v = 0;
		if(!v_.equals("")) v = Integer.parseInt(v_);
		
		if(op.equals("="))
		{
			int result = 0;
			int x = (Integer)application.getAttribute("value");
			int y = v;
			String operator = (String)application.getAttribute("operator");
			
			if(operator.equals("+"))
				result = x + y;
			else
				result = x - y;
			
			response.getWriter().printf("result is %d\n", result);
		} 
		else 
		{
			application.setAttribute("value", v);
			application.setAttribute("operator", op);
		}
		
	}
}
```

### 실험: 서로 다른 브라우저가 어플리케이션 저장소를 공유할 수 있는가

재미있는 실험을 하나 해보겠습니다. **크롬**에서 웹 페이지를 띄우고 `5`를 입력합니다. 그리고 `+` 버튼을 눌러 값을 전달합니다. 

다음으로는 **엣지**나 **파이어폭스**와 같은 브라우저를 실행해 동일한 웹 페이지로 접속합니다. 만약 여기에 `3`을 입력하고 `=` 버튼을 누르면 **서로 다른 브라우저**끼리 값을 공유할 수 있을까요?

정답은 **Yes** 입니다. 다른 브라우저에서도 이전에 입력된 값을 공유해 `8`을 출력하는 것을 확인할 수 있습니다.

## Session 객체로 상태 값 저장

앞선 예제의 `application` 을 모두 `session`으로 바꾸겠습니다. `session` 은 `HttpSession` 변수로 선언할 수 있습니다.

```java
package com.jsp_example.web;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@WebServlet("/calc2")
public class Calc2 extends HttpServlet{
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		PrintWriter out = response.getWriter();
		
		// Session
		HttpSession session = request.getSession();
		
		String v_ = request.getParameter("v");
		String op = request.getParameter("operator");
		
		int v = 0;
		if(!v_.equals("")) v = Integer.parseInt(v_);
		
		if(op.equals("="))
		{
			int result = 0;
			int x = (Integer)session.getAttribute("value");
			int y = v;
			String operator = (String)session.getAttribute("operator");
			
			if(operator.equals("+"))
				result = x + y;
			else
				result = x - y;
			
			response.getWriter().printf("result is %d\n", result);
		} 
		else 
		{
			session.setAttribute("value", v);
			session.setAttribute("operator", op);
		}
		
	}
}
```

위 코드를 실행하면 이전 Application 예제와 똑같이 동작함을 확인할 수 있습니다. 다만 한 가지 다른 점은 **서로 다른 브라우저에서 값을 공유지지 않는다**는 것입니다.

### 실험: 서로 다른 브라우저가 세션 저장소 값을 공유할 수 있는가

실제로 위 `재미있는 실험`을 진행하면 **NullPoint Error**가 발생합니다. 이는 이전 입력값이 없어 나타나는 오류입니다.

이로써 서버가 사용자를 브라우저 단위로 구분하는 것을 알게 되었습니다. 그러면 한 가지 더! 같은 크롬에서 실험해보면?

결과는 맨 처음과 같이 정상적으로 동작하는 모습을 확인할 수 있습니다. **세션**은 `브라우저`까지는 구분하나, 하위 윈도우 창을 담당하는 쓰레드는 구분하지 못 함을 알 수 있습니다.

### WAS(웹 서버)가 현재 사용자를 구분하는 방식

먼저 세션이 무엇인지 살펴보겠습니다. 세션의 사전적 정의는 **반영구적이고 상호작용적인 사용자 간의 연결상태** 또는 **그러한 논리적 연결**입니다. 즉, 여러 개체들이 연결된 **상태, 과정**이라 말할 수 있습니다.

위 정의를 떠올리며 관련 문서를 읽어보겠습니다. 아래 링크는 예제에 쓰이는 **Session Interface** 문서입니다.

**[Document]** [https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpSession.html](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpSession.html)

**HttpSession** 인터페이스는 한 개 이상의 사용자가 서버에 접속할 때, 이를 구분해주는 역할을 합니다. 이 구분된 상태를 클라이언트가 서버가 연결되었다고 생각할 수 있습니다.

서블릿 컨테이너는 HTTP 클라이언트와 HTTP 서버 사이에 세션을 생성하기 위해 **HttpSession** 인터페이스를 사용합니다. 이러한 인터페이스를 통해 형성된 세션은 아래 특징을 가지고 있습니다.

- 세션 정보는 서버의 메모리(웹 컨테이너)에 저장됩니다
- 웹 브라우저 하나당 한 개씩 할당됩니다
- 특정 시간 주기를 가집니다
- 서버로 주어지는 여러 요청을 **SID(Session ID)** 를 통해 구분합니다
- 세션 구분자, 생성 시간, 최후 접속 시간 등을 조회할 수 있습니다
- 객체를 세션에 묶어 여러 연결들이 함께 사용할 수 있도록 합니다

세션을 직접 눈으로 확인해보겠습니다. 크롬의 개발자 도구의 Network 탭의 Cookie를 캡쳐한 이미지입니다.

![session_example](/_img/2021-09-02/session_example.jpg){: .align-center}

크롬 브라우저와 엣지 브라우저는 `SESSIONID`가 서로 달라 구분될 수 있는 것입니다.

## Cookie (쿠키)

앞의 세션은 오직 서버에만 값이 저장되었습니다. 그렇다면 쿠키는 세션과 어떤 차이점을 가질까요. 

> 쿠키는 **서버**에서 정보를 심어 **클라이언트**로 정보를 전달합니다.

이러한 쿠키는 *Key* 와 *Value* 로 이뤄집니다. 이때, *Value*에는 String만 저장될 수 있어 불편하다 생각할 수 있습니다. 그러나 추후 **JSON**, **XML** 객체를 사용해 다양한 객체를 문자열로 변환하는 방법을 사용해 쿠키를 유용하게 사용하는 방법을 배울 것입니다.

서버는 **SessionID**와 **Expires 시간**을 저장해 전달하고, 추후 브라우저의 요청이 발생하면 쿠키에서 정보를 추출해 구분할 수 있는 것입니다.

<img src="/_img/2021-10-10/state_storage.jpg" style="margin-left: auto; margin-right: auto; display:block;">

우리는 위와 같이 클라이언트와 서버의 상태 저장소 모델을 간단히 요약할 수 있습니다. 보시다시피 쿠키 저장소는 클라이언트, 어플리케이션과 세션은 서버 쪽에 위치한 저장소임을 알 수 있습니다.

## 쿠키 사용하기

쿠키를 사용하는 간단한 예시는 다음과 같습니다.

- Cookie 객체를 생성해 `response.addCooke()` 로 담아 클라이언트에 전송
- 클라이언트가 가진 모든 쿠키들 중 key 값이 일치하는 것을 골라 사용

```java
// 쿠키 저장하기
String a = "1FeS";
Cookie cookie = new Cookie("tough cookie", String.valueOf(a));
response.addCookie(cookie);

// 쿠키 읽기
Cookie[] cookies = request.getCookies();
String _a = "";

if(cookies != null)
	for(Cookie _cookie: cookies)
		if("1FeS".equals(_cookie.getName()))
			_a = _cookie.getName();	
```

참고로 이런 쿠키를 저장할 때, **value 값**은 반드시 URL에 사용될 수 있는 String 형태여야 합니다. 그렇지 않으면 오류가 발생합니다. 만약 Integer 를 전달한다면 `String.valueOf(Integer)` 를 이용해 변환 후 사용해야 합니다.

이렇게 생성된 쿠키는 **Header**에 추가되어 브라우저에 전달됩니다. 전달된 쿠키는 Chrome 기준 **웹 사이트 설정 - 쿠키 - 쿠키 데이터 보기** 에서 확인할 수 있습니다.

## 예제 코드

```java
@WebServlet("/calc2")
public class Calc2 extends HttpServlet{
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		PrintWriter out = response.getWriter();

		// Session
		HttpSession session = request.getSession();
		
		// Cookie
		Cookie[] cookies = request.getCookies();
		
		String v_ = request.getParameter("v");
		String op = request.getParameter("operator");
		
		int v = 0;
		if(!v_.equals("")) v = Integer.parseInt(v_);
		
		if(op.equals("="))
		{
			int x = 0, y = v, result = 0;
			String operator = "";
			if(cookies != null)
				for(Cookie _tmp: cookies)
					if(_tmp.getName().equals("value"))
						x = Integer.parseInt(_tmp.getValue());
			
			if(cookies != null)
				for(Cookie _tmp: cookies)
					if(_tmp.getName().equals("op"))
						operator = _tmp.getValue();
			
			if(operator.equals("+"))
				result = x + y;
			else
				result = x - y;
			
			response.getWriter().printf("result is %d\n", result);
		}
		else 
		{
			Cookie valueCookie = new Cookie("value", String.valueOf(v));
			Cookie opCookie = new Cookie("op", op);
			response.addCookie(valueCookie);
			response.addCookie(opCookie);
		}
	}
}
```

## 사용자 옵션 쿠키

쿠키는 다음과 같은 옵션들이 있습니다. 우리는 쿠키를 만들 때, 다음과 같은 옵션들을 설정해 클라이언트와 주고받을 수 있습니다.

<img src="/_img/2021-10-10/CookieOption.jpg" style="display: center;">

### Cookie option: path

쿠키는 요청이 전달되는 경로에 따라 쿠키 수신 여부를 결정할 수 있습니다. 아래 `path`에 개발자가 원하는 경로를 설정하면 하위 `path`로 요청이 전달되는 경우, 브라우저의 쿠키를 수신할 수 있습니다.

```
cookieObject.setPath("path");
```

우리는 다양한 기능을 하는 서블릿을 개발합니다. 브라우저가 가진 쿠키가 전송되었을 때, 모든 다양한 서블릿들이 모든 쿠키를 수신한다면 같은 key 값, 중복된 value 등의 문제로 비정상적인 동작을 수행할 수 있습니다. 

따라서 위의 **path** 옵션을 활용해 요청된 리소스(servlet & web document)가 존재하는 URL 경로에만 쿠키를 보낼 수 있도록 합니다.

정리하자면, **특정 경로에서만 쿠키를 추가하기를 원할 때** Path를 사용합니다. 쿠키를 구울 때, `~한 경로를 가진 곳이 아니면 넌 전송되지 마!` 라고 미리 명령해놓는 것이라 비유할 수 있습니다.


### Cookie option: maxAge

`maxAge` 옵션은 브라우저가 꺼져도 쿠키가 만료되지 않도록 합니다. `seconds`가 해당하는 자리에 정수값을 입력하면 초 단위로 만료 날짜를 설정할 수 있습니다.

```
cookieObject.setMaxAge(seconds);
```

## 정리

|저장공간|Application|Session|Cookie|
|---|---|---|---|
|사용범위|전역 범위에서 사용하는 저장 공간|세션 범위에서 사용하는 저장 공간|Web Browser 별 지정한 path 범주 공간|
|생명주기|WAS가 시작해서 종료할 때 까지|세션이 시작해서 종료할 때 까지|Browser에 전달한 시간부터 만료 시간까지|
|저장위치|WAS 서버의 메모리|WAS 서버의 메모리|Web Browser 의 메모리 또는 파일|