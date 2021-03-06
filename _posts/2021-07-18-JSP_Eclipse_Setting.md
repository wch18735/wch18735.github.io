---
title: "[JSP] Eclipse 이용한 웹 개발환경 설정"
excerpt: "Eclipse for JSP development"
date: 2021-07-18
layout: single
classes: wide
category:
 - jsp
tag:
 - java
 - jsp
 - servlet
 - eclipse
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Eclipse 를 이용한 개발환경 설정

이틀 정도 vscode에 JSP 환경설정하는 방법을 찾아다니며 노력했지만 제대로 되지 않아 결국 Eclipse를 설치했습니다. 아래는 개발 환경 구성 중 강좌와 다른 몇 가지 부분들을 해결한 참고 사이트입니다.

- [이클립스 설치](https://www.eclipse.org/downloads/)
- [Dynamic Web Project 없을 때](https://ssimplay.tistory.com/204)
- [Server Runtime Environment tomcat missing](https://stackoverflow.com/questions/2000078/apache-tomcat-not-showing-in-eclipse-server-runtime-environments)

Eclipse 장점은 인터넷 상에 자료가 풍부하다는 것이라는데, 확실히 vscode 보다 한 가지 문제에 대한 여러 해결방법이 존재했습니다. StackOverflow 만 검색해도 환경 설정은 어렵지 않았습니다.

단점은 조금 느리다는 점이 있습니다. 설정을 진행하고 저장할 때, 확실히 JVM 위에서 돌아가는 프로그램이라 작업이 완료되기까지 응답없음 또는 멈춤 현상이 발생합니다.

## Annotation을 통한 URL 매핑

**Eclipse** 를 사용하게 되면 아래 과정을 한 번에 진행할 수 있습니다.

- java code 작성
- java code 컴파일
- web.xml에 byte code 등록
- tomcat 서버 재시작

단순히 ctrl + F11 키를 눌러주면 지금까지 했던 모든 과정이 한 번에 진행됩니다. 한 가지 더 편리한 점이 있습니다. 이는 servlet 3.0 이상에서 지원되는 기능으로 `@WebServlet('/URL')` 어노테이션을 통해 직접 매핑 코드를 작성하지 않아도 지정한 URL을 받으면, 이전에 컴파일 해두었던 서블릿 코드가 실행됩니다.

```java
package com.jsp_example.web;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/hello")
public class FirstServlet extends HttpServlet{
	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// TODO Auto-generated method stub
		PrintWriter out = resp.getWriter();
		out.println("Hello World!");
	}
}
```

위의 코드를 Eclipse 내에서 동작시키면 다음과 같은 화면이 나타납니다. 

![jsp hello](/_img/2021-07-12/jsp_hello.jpg){: .align-center}

정말 편리하네요. vscode 역시 여러 강력한 장점들이 많지만, 특정 언어에 많이 종속되는 작업은 그만큼 많은 사람들에게 검증된 IDE를 사용하는 편이 좋다는 것을 배웠습니다. vscode는 조금 작은 작업이나 블로그 글 작성, 스닙 코드 등을 동작시켜보는 방향으로 사용해야겠다고 생각했습니다.