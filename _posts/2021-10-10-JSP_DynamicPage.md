---
title: "[JSP] Dynamic page / Server page 작성 예제"
excerpt: "동적인 페이지(서버 페이지) 작성"
date: 2021-10-10
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

# 동적 웹페이지 작성 필요성

동적 웹페이지란 쉽게 말해 `사용자 행위에 따라 모습이 바뀌는 페이지`를 뜻합니다. 물론 클라이언트 단에서 이를 적용할 수 있겠지만, 지금은 서버에 요청이 전달되었을 때, 이를 이용해 **새로운 페이지를 작성**하여 **브라우저에 전달** 하는 것을 다루겠습니다.

<img src="/_img/2021-10-10/dynamic_page.jpg" style="left-margin: auto; right-margin: auto; display:block;">

4번 동적 웹피이지 생성은 서버에서 이루어집니다. 이는 선택적 과정으로, 계산기와 같이 현재까지 입력한 결과를 실시간으로 보여주기 위해서 활용됩니다. 즉, 요청에 대한 결과 값을 **HTML 문서에 꽂아** 전달하는 방식입니다.

## 계산기 만들기

윈도우 계산기와 비슷하지만 기능은 사칙연산으로 제한되는 어플리케이션을 만들어봅니다. 이 과정을 통해 동적 웹페이지 생성과 관련된 내용을 얻을 수 있을 것 같습니다.

### 화면 구성

먼저 아래와 같이 웹 상에 계산기를 만들어 보겠습니다.

<img src="/_img/2021-10-10/calculator.jpg" style="margin: auto auto; display: block;">

이를 만드는 HTML 문서 내용은 아래 **calc3.html** 에 추가해 놓았습니다. 복사해서 붙여넣은 뒤 사용하셔도 좋습니다.

## 서블릿을 이용한 동적 페이지 작성

프로젝트 src에 `Calc3.java`, `CalcPage.java`, webapp 페이지에 `calc3.html` 를 추가합니다. 그리고 `CalcPage.java`를 다음과 같이 세팅합니다.

```java
@WebServlet("/calcpage")
public class CalcPage extends HttpServlet{
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8;");
		PrintWriter out = response.getWriter();
		
		out.write("html code");
		}
}
```

위의 코드에서 `html code` 라고 씌여진 곳에 위의 코드를 복사해 추가하고 F11을 눌러보면 동일한 페이지가 출력되는 것을 확인할 수 있습니다. 이로써 `.java` 파일을 실행하는 것만으로도 페이지를 표현할 수 있는 것을 확인했습니다.

이 개념을 확장해 동적인 페이지를 제작할 수 있습니다. java file 안에서는 여러 연산이 가능합니다. 그리고 `out.printf()` 는 출력 형식자를 지정하는 기능을 지원합니다.

이로써! 우리가 원하는 부분에 **계산 결과**를 집어 넣어 전달할 수 있는 것입니다. 실제 예시 코드를 작성해보겠습니다. 아래 **CalcPage.java** 에 `out.printf()` 를 넣는 코드를 추가했습니다. 원래 강좌에서 진행되는 과정은 시간이 너무 오래걸려 중간만 바꾸는 방법으로 바꾸었습니다.

## 서블릿 계산기

### Calc3.java

`calc3.html` 의 `action="calc3"` 으로 `@WebServlet("/cakc3")` 처리된 서블릿이 동작합니다. 이곳에서는 사용자가 클릭한 내용을 얻어와 표현할 문자열 `expression` 에 누적해 전달합니다.

- 쿠키 지우기
```java
// Cookie 생성
    Cookie expCookie = new Cookie("expression", expression);
    if(operator != null && operator.equals("C")) expCookie.setMaxAge(0);
```
위와 같이 `setMaxAge(0);`를 통해 쿠키가 전송되자마자 삭제되도록 만들 수 있습니다.

- servlet 재실행
```java
response.sendRedirect("calcpage");
```
`response.sendRedirect()` 는 HTML 페이지와 더불어 **Servlet** 으로의 우회도 가능합니다. 해당 메소드를 가장 마지막에 호출해주어 **calcpage** 서블릿을 실행해줍니다. 이때, **redirect**는 브라우저에서 해당 서블릿을 호출한 것과 동일한 효과를 내기 때문에 방금 전에 보낸 Cookie를 수신할 수 있습니다.

아래는 위 내용을 종합해 작성된 서블릿 소스 코드입니다.

```java
package com.jsp_example.web;

import java.io.IOException;
import java.io.PrintWriter;

import javax.script.ScriptEngine;
import javax.script.ScriptEngineManager;
import javax.script.ScriptException;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@WebServlet("/calc3")
public class Calc3 extends HttpServlet{
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		Cookie[] cookies = request.getCookies();
		
		String value = request.getParameter("value");
		String operator = request.getParameter("operator");
		String dot = request.getParameter("dot");
		
		String expression = "";
		if(cookies != null) {
			for(Cookie c : cookies) {
				if(c.getName().equals("expression")) {
					expression = c.getValue();
					break;
				}
			}
		}
		
		if(operator != null && operator.equals("=")) {
			ScriptEngine engine = new ScriptEngineManager().getEngineByName("nashorn");
			try {
				expression = String.valueOf(engine.eval(expression));
			} catch (ScriptException e) {
				e.printStackTrace();
			}
		}
		else if(operator != null && operator.equals("C")) {
			expression = "";
		}
		else {
			expression += (value == null) ? "" : value;
			expression += (operator == null) ? "" : operator;
			expression += (dot == null) ? "" : dot;
		}
		
		Cookie expCookie = new Cookie("expression", expression);
		if(operator != null && operator.equals("C")) expCookie.setMaxAge(0);
		
		response.addCookie(expCookie);

		response.sendRedirect("calcpage");
	}
}
```

### CalcPage.java

```java
package com.jsp_example.web;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@WebServlet("/calcpage")
public class CalcPage extends HttpServlet{
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		Cookie[] cookies = request.getCookies();
		
		String expression = "0";
		if(cookies != null) {
			for(Cookie c : cookies) {
				if(c.getName().equals("expression")) {
					expression = c.getValue();
					break;
				}
			}
		}
		
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8;");
		PrintWriter out = response.getWriter();
		
		/* 엄청나게 긴 출력문 */
		}
}
```

코드 내부에 `out.write()` 를 이용한 엄청나게 긴 출력문이 삽입됩니다. 웹 화면에 html 소스를 출력하기 위함입니다. 다행히 화면을 출력할 때, 매번 이렇게 `print` 또는 `write` 를 사용하지 않아도 되는 방법이 존재합니다. 바로 다음 포스트에서 다룰 *Jasper* 입니다. 

### calc3.html

```html
<style>
	input{
		width: 50px;
		height:50px;
	}
	
	.output {
		height: 50px;
		background: #e9e9e9;
		font-size: 24px;
		font-weight: bold;
		text-align: right;
		padding: 0px 5px;
	}
</style>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
 <form action="calc3" method="post">
		<table>
			<tr>
				<td colspan="4" class="output">0</td>
			</tr>
			<tr>
				<td><input type="submit" name="operator" value="CE"/></td>
				<td><input type="submit" name="operator" value="C"/></td>
				<td><input type="submit" name="operator" value="←"/></td>
				<td><input type="submit" name="operator" value="÷"/></td>
			</tr>
			<tr>
				<td><input type="submit" name="value" value="7"/></td>
				<td><input type="submit" name="value" value="8"/></td>
				<td><input type="submit" name="value" value="9"/></td>
				<td><input type="submit" name="operator" value="×"/></td>
			</tr>
			<tr>
				<td><input type="submit" name="value" value="4"/></td>
				<td><input type="submit" name="value" value="5"/></td>
				<td><input type="submit" name="value" value="6"/></td>
				<td><input type="submit" name="operator" value="-"/></td>
			</tr>
			<tr>
				<td><input type="submit" name="value" value="1"/></td>
				<td><input type="submit" name="value" value="2"/></td>
				<td><input type="submit" name="value" value="3"/></td>
				<td><input type="submit" name="operator" value="+"/></td>
			</tr>
			<tr>
				<td><input type="submit" name="operator" value=""/></td>
				<td><input type="submit" name="value" value="0"/></td>
				<td><input type="submit" name="dot" value="."/></td>
				<td><input type="submit" name="operator" value="="/></td>
			</tr>
		</table>
		
		<div>
			result: 0
		</div>
	</form>
</body>
</html>
```

## GET과 POST 요청에 대한 서비스 메서드 구현

위에서 모든 기능은 **HttpServlet**의 **service()** 함수 안에 구현되어 있습니다. 

`GET` 또는 `POST` 요청에 따른 서로 다른 동작을 보여주려면 어떻게 해야 할까요. 모든 요청 형태에 따라 항상 **service()** 메서드를 동작시키고 내부에서 분기를 시키는 과정을 거쳐야 할까요. 아래처럼?

```java
@WebServlet("/calculator")
public class Calculator extends HttpServlet{
	@Override
	public void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		if(request.getMethod().equals("GET")) {
			System.out.println("GET!\n");
		} else if (request.getMethod().equals("POST")) {
			System.out.println("POST!\n");
		}
	}
}
```

아닙니다.

처음 Override 를 제공받을 때, `super.service(req, res);` 가 자동으로 생성되는 것을 확인했을 겁니다. 이 메서드가 Override 가 되지 않았다면 해당 `요청에 따라` `아래 두 메서드 중 하나`를 찾아 동작시킵니다.

- **GET :** doGet()
- **POST :** doPost()

따라서 우리는 service() 메서드를 사용하지 않고, 곧바로 요청에 따른 동작을 처리할 수 있습니다.

```java
@WebServlet("/calculator")
public class Calculator extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// TODO Auto-generated method stub
		super.doGet(req, resp);
	}
	
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// TODO Auto-generated method stub
		super.doPost(req, resp);
	}
}
```

이를 이용해 위에서 만들었던 계산기 웹 어플리케이션을 하나의 서블릿으로 합치는 리팩토링이 가능해집니다.