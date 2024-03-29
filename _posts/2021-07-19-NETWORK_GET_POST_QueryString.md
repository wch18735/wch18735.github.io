---
title: "[Network] GET 요청과 POST 요청의 차이"
excerpt: "GET & POST Requests"
date: 2021-07-19
layout: single
classes: wide
category:
 - network
tag:
 - java
 - jsp
 - servlet
 - get
 - post
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# HTTP 메소드 (GET, POST)

**HTTP(Hyper Text Transfer Protocol)** 란 웹 상에서 클라이언트와 서버 간에 요청/응답으로 데이터를 주고 받을 수 있는 프로토콜을 뜻합니다. 클라이언트가 프로토콜을 통해 서버에게 요청을 보내면 서버는 요청에 맞는 응답을 클라이언트에게 전송합니다.

이때, HTTP 요청에 포함되는 메소드는 서버가 요청을 수행하기 위해 필요한 **정보**를 전달해 특정 작업을 지시합니다. 이때, **GET**과 **POST** 메소드가 존재합니다. [이곳](https://hongsii.github.io/2017/08/02/what-is-the-difference-get-and-post/)에서 더 자세한 내용을 확인할 수 있습니다.

## GET

**GET**은 **서버로부터 정보를 조회**하기 위해 설계된 메소드입니다. 요청을 전송할 때 필요한 데이터를 Body에 담는 대신 쿼리스트링을 통해 전송합니다. **URL(Uniform Resource Locator)** 의 끝에 `?`와 함께 이름과 값으로 쌓을 이루는 요청 파라미터를 쿼리 스트링이라 부릅니다.

요청 파라미터가 여러 개라면 `&`로 연결합니다. 아래는 간단한 예시입니다. 요청 파라미터 명은 name1, name2 이고 각 인자에 해당하는 값은 value1, value2 라는 값으로 서버에 요청을 보내게 됩니다.

`www.example-url.com/resources?name1=value1&name2=value2`

이를 함수로 나타내보면 다음과 같습니다.

```python
def function_in_server(value1, value2):
    name1 = value1
    name2 = value2

    # perform with name1 and name2
    # ...
```

## POST

**POST**는 **리소스를 생성/변경하기 위해 설계**되었기 때문에 GET과 달리 전송해야 될 데이터를 HTTP 메세지의 Body에 담아 전송합니다. HTTP 메세지의 Body는 길이 제한 없이 데이터를 전송할 수 있습니다. 이러한 특징으로 POST는 대용량 데이터를 전송할 수 있습니다.

POST는 데이터가 Body로 전송되고 내용이 눈에 보이지 않아 GET보다 보안적인 면에서 안전하다고 생각할 수 있습니다. 그러나 POST 요청도 크롬 개발자 도구, Fiddler 같은 툴로 요청 내용을 확인할 수 있기 때문에 민감한 데이터는 반드시 암호화해 전송해야 한다고 합니다.

또한 POST 요청시 요청 헤더에 Content-Type을 표시해야 합니다. 데이터 타입을 표시하지 않으면 서버는 내용이나 URL에 포함된 리소스 확장자명 등으로 데이터 타입을 유추한다고 합니다. 이전 포스팅에서 `res.setContentType()` 을 통해 클라이언트에 타입을 전송했던 것과 마찬가지로 서버에서도 자료형을 추측할 수 있도록 타입을 기재하는 것입니다. 만약 서버에서 데이터 타입을 알 수 없는 경우에는 `application/octet-stream`으로 요청을 처리한다고 합니다.

## GET과 POST 차이

간단하게 말하면 동일한 연산을 여러 번 수행했을 때 같은 응답이 돌아오는가? 입니다. 만약 같은 동작이 보장되면 **Idempotent**라 하고, 그 반대는 **Non-idempotent**라 합니다. 즉, GET 메소드는 서버의 정보를 바꾸지 않기 때문에 Idempotent 라고 할 수 있으며, POST는 상태를 변화시키기 때문에 **Non-idempotent**라 합니다.

게시글을 쓰면 게시글이 저장되고, 등록된 글을 다시 수정하는 등에 사용됩니다. 서버가 가진 데이터에 변경을 주는 방법으로는 아래 네 가지 메소드가 기능별로 정의됩니다.

- 생성: POST
- 수정: PUT / PATCH
- 삭제: DELETE

# GET 요청 예제

이번 포스팅에서는 두 가지 메소드 중 GET 메소드를 사용하는 예제를 간략하게 다뤄보려고 합니다.

## response.getParameter()

아래 코드에는 `cnt` 라는 약속된 파라미터에 값을 전달합니다. `ctrl + F11` 눌러서 서버를 재시작 후 `?` 를 통해 값을 전달해 보겠습니다.

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
		
		int cnt = Integer.parseInt(req.getParameter("cnt"));
		
		for(int i = 0; i < cnt; i++)
			out.println((i + 1) + ": 안녕 Servlet!! </br>");
	}
}
```

다음과 같이 전달된 인자 값에 대해 성공적으로 for문이 돌아가는 것을 확인할 수 있습니다.

![cnt parameter](/_img/2021-07-19/cnt_5.jpg){: .align-center}

그런데 **PlusServlet.java** 에서 바로 컴파일을 진행하면 에러가 발생하는 경우가 있습니다. 바로 `?`를 통해 `cnt` 값으로 인자 값이 전달되지 않을 때입니다. 이런 경우를 방지하기 위해 우리는 기본 값을 사용할 수 있습니다.

## 기본 값 사용

쿼리 스트링은 다음과 같은 경우들이 있을 수 있습니다.

- case1: `http://localhost:8080/plus?cnt=5`
- case2: `http://localhost:8080/plus?cnt=`
- case3: `http://localhost:8080/plus?`
- case4: `http://localhost:8080/plus`

각각의 경우에 대한 묵시적 약속 값이 있어야 오류가 발생하지 않습니다. 아래 코드와 같이 **default** 값을 지정하고, 공백이 입력됐을 때, 아무것도 입력되지 않았을 때를 경우에 따라 나누도록 합니다.

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
		
		String cnt_ = req.getParameter("cnt");
		
		int cnt = 10;
		if(cnt_ != null && !cnt_.equals(""))
			cnt = Integer.parseInt(cnt_);
		
		for(int i = 0; i < cnt; i++)
			out.println((i + 1) + ": 안녕 Servlet!! </br>");
	}
}
```

위의 코드는 초기 값으로 10을 가지고 있습니다. 그리고 `req.getParameter("cnt")`를 통해 입력 값이 공백이 아니고, 빈 문자열이 아닐 경우에 `parseInt()` 해줍니다. 이후에는 에러 없이 서버가 재시작 되고 쿼리에 따라 동작하는 것을 볼 수 있습니다.

![query string default](/_img/2021-07-19/query_string_default.gif){: .align-center}

## href="URL?QUERY_STRING"

사용자가 위와 같이 쿼리 스트링을 입력하여 서버에 GET 메소드를 호출하는 경우는 거의 없습니다. 이는 보통 사용자가 버튼을 클릭하거나 입력 등의 동작을 했을 때 자동으로 서버로 전송되도록 설계됩니다.

아래와 같은 html 문서를 보겠습니다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	환영합니다.
    <a href="hello">인사하기</a>
    <a href="hello?cnt=3">인사하기3</a>
</body>
</html>
```

위의 코드에서 첫 번재 `a 태그`를 선택하면 초기 값인 10번, 아래 **인사하기3**는 3번을 동작시킵니다. 이것이 가장 기본적인 형태의 GET 요청입니다.