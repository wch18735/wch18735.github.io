---
title: "[JSP] POST 한글 입력 문제 해결"
excerpt: "POST Request scripted in korean"
date: 2021-07-23
layout: single
classes: wide
category:
 - jsp
tag:
 - java
 - jsp
 - servlet
 - korean
 - post
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# POST 한글 입력 문제

이전 실습 문서에서 한글을 전송해보면 아래와 같은 오류가 발생합니다.

![korean](/_img/2021-07-23/korean.jpg){: .align-center}

이 오류는 인코딩 방식 차이에 의해 발생합니다. 한글은 **UTF-8**과 같은 멀티 바이트 인코딩을 사용합니다. 한글의 경우 <span style="color: red; font-weight: bold">2Byte</span>가 한 문자입니다.

그러나 톰캣의 기본 인식 방법은 **ISO-8859-1**으로 <span style="color: red; font-weight: bold">1Byte</span>를 한 문자로 사용합니다.

따라서 이를 처리하는 서버에서 UTF-8로 요청을 받아들여야 합니다. 톰캣 인코딩 방식을 바꾸는 방법은 두 가지 입니다. 첫째는 **server.xml** 의 **Connector** 설정을 변경하는 것입니다.

그러나 하나의 톰캣은 여러 서비스를 운영할 수 있습니다. 따라서 기본 설정을 건드리기 보다는 서블릿 하나의 인코딩 방식을 지정해주는 방법이 적절합니다.

요청에서 인자를 받아오기 전 `request.setCharacterEncoding("UTF-8")` 로 바꿔줍니다.

```java
package com.jsp_example.web;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/notice-reg")
public class NoticeReg extends HttpServlet{
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// default settings (Server encoding, Client encoding)
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8");
		
		// request encoding
		request.setCharacterEncoding("UTF-8");
		
		// out instance
		PrintWriter out = response.getWriter();
		
		// out target
		String title = request.getParameter("title");
		String content = request.getParameter("content");
		
		
		// print
		out.println(title);
		out.println(content);
	}
}
```

이렇게 서블릿을 수정한 뒤 실행하겠습니다. 성공적으로 수행되는 것을 확인할 수 있습니다.

![korean](/_img/2021-07-23/korean_success.jpg){: .align-center}

