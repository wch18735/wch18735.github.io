---
title: "[JSP] POST 요청 예제"
excerpt: "POST Request"
date: 2021-07-22
layout: single
classes: wide
category:
 - jsp
tag:
 - java
 - jsp
 - servlet
 - post
 - post request
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# POST Request

이전에는 쿼리 스트링을 이용해 요청하는 법을 알아보았습니다. 이번에는 입력할 내용이 많을 때, 일정한 양식을 통해 전달하는 **POST**를 알아보고 실습합니다.

HTTP `POST` 메소드는 서버로 데이터를 전송합니다. 이때, 요청 본문 유형은 `content-type` 헤더로 나타냅니다. 이전 포스팅에서 다뤘던 `PUT`과는 [멱등성](https://developer.mozilla.org/ko/docs/Glossary/Idempotent)이라고 합니다. 간단하게 정리하면 동일한 동작에 대해 동일한 결과를 보여주는가에 대한 성질입니다.

`POST` 요청은 보통 HTML 양식을 통해 서버에 전송하여 서버에 변경사항을 만듭니다. 이 경우 콘텐츠 유형은 `<form>` 요소의 `enctype` 특성이나 `<input>`, `<button>` 요소의 `formenctype` 특성 안에 적당한 문자열을 넣어 결정합니다.

또한 POST 요청은 HTML 양식이 아닌 다른 방법으로 전송할 수 있습니다. 그 대표적인 예로는 **XMLHTttpRequest**가 있습니다. 이 경우에는 요청의 본문이 어떤 형태도 취할 수 있습니다.

## 예제 코드

POST 실습을 위한 `html` 문서를 다음과 같이 작성합니다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<div>
		<form action="notice-reg">
			<div>
				<label>제목:</label><input name="title" type="text">
			</div>
			<div>
				<label>내용:</label>
				<textarea name="content"></textarea>
			</div>
			<div>
				<input type="submit" value="등록" />
			</div>
		</form>
	</div>
</body>
</html>
```

해당 문서에는 두 가지 변수가 있습니다. 바로 **title, content** 입니다. 서버로 요청된 두 파라미터를 수신해 브라우저에 출력하는 서블릿을 작성해보겠습니다.

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

이 두 코드는 아래와 같은 관계를 가지고 있습니다.

![relationship](/_img/2021-07-22/relation_with_html.jpg){: .align-center.jpg}

`@WebServlet` **Annotation**을 이용해 **web.xml** 내부에 직접 링크를 걸지 않아도 되도록 도와줍니다. 이는 **action="notice-reg"** 와 매칭됩니다. 해당 요청이 전송되면 해당 서블릿이 동작하게 됩니다.

코드 내부에 기재한 `name="title"` 과 `name="content"` 는 **Parameter**의 이름으로 개발자는 서블릿 내부에서 이와 동일한 이름의 변수를 읽어와 사용할 수 있습니다.

이를 실행하면 아래와 같은 화면을 볼 수 있습니다.

![html1](/_img/2021-07-22/html1.jpg){: .align-center}

이를 실행하면 입력한 결과가 크롬 브라우저에 출력됩니다. 이때, 주소창에 입력했던 값들이 나타나는 것을 볼 수 있습니다.

![html2](/_img/2021-07-22/html2.jpg){: .align-center}

이때, 주소창에 쿼리 스트링이 나타나는 것을 볼 수 있습니다. 만약 **내용**에 매우 긴 내용이 들어가면 그만큼 길이가 길어질 수 있습니다. 이를 방지하는 방법이 이를 `POST`로 보내는 것입니다.

HTML 문서의 **action**에 `method="post"` 를 추가해주고 다시 실행해줍니다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<div>
		<form action="notice-reg" method="post">
			<div>
				<label>제목:</label><input name="title" type="text">
			</div>
			<div>
				<label>내용:</label>
				<textarea name="content"></textarea>
			</div>
			<div>
				<input type="submit" value="등록" />
			</div>
		</form>
	</div>
</body>
</html>
```

다음과 같이 뒤에 붙던 쿼리 스트링이 사라진 것을 보실 수 있습니다.

![html3](/_img/2021-07-22/html3.jpg){: .align-center}

데이터가 어떻게 전송되는지는 F12 개발자 도구를 눌러 확인 가능합니다. 요청과 응답을 나타내는 그래프와 더불어 헤더 정보, 어떤 데이터가 소스로 전송되었는지 자세히 알 수 있습니다.