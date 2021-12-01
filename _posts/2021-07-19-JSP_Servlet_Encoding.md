---
title: "[JSP] 서블릿 출력 형식을 지정해야하는 이유"
excerpt: "Servlet format with setCharcterEncoding() and setContentType()"
date: 2021-07-19
layout: single
classes: wide
category:
    - jsp
tag:
    - java
    - jsp
    - servlet
	- set character
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 브라우저별 출력 차이

아래 코드를 실행해보도록 하겠습니다.

```java
package com.jsp_example.web;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/plus")
public class PlusServlet extends HttpServlet{
	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// TODO Auto-generated method stub
		
		PrintWriter out = resp.getWriter();
		
		for(int i = 0; i < 10; i++)
			out.println((i + 1) + ": Hello Servlet!!");
	}
}
```

위의 결과는 Eclipse 내에서 실행되는 결과이고, 아래는 Chrome 브라우저 내에서 실행되는 결과입니다. `println()` 출력 메소드에 대해 위의 Edge 브라우저에서는 계행이 이뤄지지 않았고, 아래 Chrome 브라우저에서는 계행이 이뤄졌습니다. 

이는 각 브라우저가 문서를 자의적으로 해석하기 때문에 발생하는 현상입니다. 컨텐츠 형식이 지정되지 않은 문서를 응답받으면 Edge는 HTML로, Chrome은 TXT로 해석하기 때문에 화면에 출력되는 형태가 달라지는 것입니다.

![browser difference](/_img/2021-07-19/browser_difference.jpg){: .align-center}

# 컨텐츠 형식

이 문제를 해결하려면 브라우저에 **컨텐츠 형식**을 알려줘야 합니다. 여기에 한 가지 더해 한글을 출력하는 서블릿을 아래와 같이 작성하고 동작시켜 보겠습니다.

```java
package com.jsp_example.web;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/plus")
public class PlusServlet extends HttpServlet{
	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// TODO Auto-generated method stub
		
		PrintWriter out = resp.getWriter();
		
		for(int i = 0; i < 10; i++)
			out.println((i + 1) + ": 안녕 Servlet!!");
	}
}
```

아래와 같이 한글이 들어간 부분이 물음표로 출력되는 것을 볼 수 있습니다. 우리는 이를 보통 `깨졌다` 라고 표현합니다.

![browser difference](/_img/2021-07-19/korean.jpg){: .align-center}

이렇게 한글이 깨지는 이유는 아래 두 가지입니다.

- 서버에서 **한글을 지원하지 않는 문자코드**로 인코딩한 경우
- 서버에서는 **UTF-8로 인코딩**해서 보냈지만 브라우저가 **다른 인코딩 코드**로 해석한 경우

보통의 서버는 형식을 지정하지 않으면 ISO-8859-1 형식으로 문서를 인코딩합니다. 서버는 이 인코딩된 데이터를 클라이언트로 1byte 씩 전송합니다. 그러나 한글 한 글자는 2byte로 인코딩되기 때문에 오류가 발생합니다.

반면 UTF-8 형식으로 인코딩까지 마치고 2byte씩 제대로 보낸 경우에도 오류가 발생할 수 있습니다. 바로 클라이언트에서 해석할 때 1byte 씩 해석하는 경우입니다.

## UTF-8 인코딩

먼저 UTF-8 로 인코딩해서 보내보겠습니다. 우리는 특정 인코딩 방식을 응답에 적용할 수 있습니다.

```java
package com.jsp_example.web;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/plus")
public class PlusServlet extends HttpServlet{
	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// TODO Auto-generated method stub
		resp.setCharacterEncoding("UTF-8");
		PrintWriter out = resp.getWriter();
		
		for(int i = 0; i < 10; i++)
			out.println((i + 1) + ": 안녕 Servlet!!");
	}
}
```

`resp.setCharacterEncoding("UTF-8")` 을 추가하니 아래와 같은 결과가 나타났습니다. **?** 였던 한글들이 **뷃◆** 등으로 바뀌었습니다. 이는 2byte 인코딩은 제대로 적용되었으나 클라이언트 브라우저가 해석을 잘 못 한 경우입니다.

![browser difference](/_img/2021-07-19/wrong_in_client.jpg){: .align-center}

이를 Chrome 브라우저에서 열고 F12를 눌러 개발자 환경으로 들어가 보겠습니다. 거기에서 Network 창을 열어주고 새로 고침을 하면 아래와 같은 화면을 볼 수 있습니다.

![browser difference](/_img/2021-07-19/response_header.jpg){: .align-center}

아래 **Response Header** 에는 생성된 날짜, 길이 등이 기재되어 있습니다. 저 부분에 **어떻게 읽을 것**을 삽입해주면 클라이언트에서도 서버에서 인코딩한 방식 그대로 해석할 수 있을 것입니다.

## 컨텐츠 타입 추가

```java
package com.jsp_example.web;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/plus")
public class PlusServlet extends HttpServlet{
	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// TODO Auto-generated method stub
		resp.setCharacterEncoding("UTF-8");
		resp.setContentType("text/html; charset=UTF-8");
		PrintWriter out = resp.getWriter();
		
		for(int i = 0; i < 10; i++)
			out.println((i + 1) + ": 안녕 Servlet!!");
	}
}
```

위와 같이 `resp.setContentType()`으로 문서의 타입과 인코딩 방식을 정해줍니다. 이로써 서로 다른 클라이언트 브라우저에 **동일한 해석 형식**을 제공할 수 있게 되었습니다. 직접 실행해보면 아래와 같이 크롬, 엣지에서 같은 출력 결과를 보여줍니다.

![browser difference](/_img/2021-07-19/content_type.jpg){: .align-center}