---
title: "[JSP] Servlet 필터"
excerpt: "Servlet filter"
date: 2021-07-25
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

# 서블릿 필터 (Servlet filter)

이전 포스트는 Servlet 내부에서 설정 바꾸는 방법을 다뤘습니다. 만약 모든 요청을 `UTF-8`로 받고싶다면, 모든 서블릿에 `request.setCharacterEncoding("UTF-8");` 코드를 추가해야 하는가와 같은 의문이 생깁니다.

요청을 받거나 응답을 보낼 때 선행적으로 실행되어야 할 작업들을 모아 놓는 것을 필터라고 합니다. 

필터를 사용하면 매 서블릿 클래스마다 응답의 인코딩 형식을 추가하지 않아도 됩니다. 또한 모든 서블릿에 적용할 수 있는 공통 속성이 되기 때문에 관리 역시 수월해집니다.

![was client server](/_img/2021-07-25/server_client.jpg){: .align-center}

위 그림에서 볼 수 있듯 **서블릿 컨테이너** 에서 서블릿을 실행하기 전 필터를 거쳐 선행 작업을 수행할 수 있습니다. 이러한 필터는 요청 작업과 응답 작업에 모두 적용 가능합니다.

## 서블릿 필터 추가 (Add servlet filter)

웹 패키지 하위에 필터 패키지를 추가하고 `CharacterEncodingFilter` 클래스를 생성합니다. 이때, **Interface** Add 키를 누르고 `Filter - javax.servlet` 을 추가해 줍니다.

![was client server](/_img/2021-07-25/add_filter.jpg){: .align-center}

이렇게 설정된 필터는 다음과 같은 경우 실행됩니다.

- 톰캣이 처음 실행될 때
- 요청이 발생할 때

## 필터 등록 (Filter registration with annotation)

필터 역시 **web.xml** 파일에 매핑을 추가하여 사용했습니다. 이전 실습에서 느끼셨겠지만 이 방법은 **error-prone** 하며 번거롭습니다. 현재는 **Servlet**과 마찬가지로 어노테이션을 이용해 등록할 수 있습니다.

```java
package com.jsp_example.web.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;

@WebFilter("/*")
public class CharacterEncodingFilter implements Filter {

	@Override
	public void doFilter(ServletRequest request, 
			ServletResponse response, 
			FilterChain chain)
			throws IOException, ServletException {
		
		request.setCharacterEncoding("UTF-8");	
	}
}
```

`@WebFilter()` 내부에 원하는 URL 형태를 입력합니다. 위와 같이 표기하면 모든 URL에 대해 필터링 하겠다는 뜻으로 해석됩니다.

## 필터 체인 (Filter chain)

웹을 실행해 보겠습니다. 어떠한 화면도 나타나지 않는데, 이유는 요청과 응답이 필터로부터 **전달**되지 않았기 때문입니다.

`chain.doFilter(request, response);` 를 통해 요청과 응답을 전달할 수 있습니다.

```java
package com.jsp_example.web.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;

@WebFilter("/*")
public class CharacterEncodingFilter implements Filter {

	@Override
	public void doFilter(ServletRequest request, 
			ServletResponse response, 
			FilterChain chain)
			throws IOException, ServletException {
		
		request.setCharacterEncoding("UTF-8");
		chain.doFilter(request, response);
	}
}
```

위와 같이 `chain.doFilter(request, response);` 까지 추가하고, 기존 서블릿에서 설정 코드를 지워봅니다. 그러면 기존 서블릿에서 요청 인코딩 설정 없이도 웹이 정상적으로 동작함을 확인할 수 있습니다.

![was client server](/_img/2021-07-25/hello.jpg){: .align-center}

# 내용 정리

서블릿은 네트워크 통신 사이에 특별한 동작을 수행합니다. 홈페이지 접속 전 이벤트 창 보이기, 전달되는 데이터 인코딩 등의 작업이 그러합니다. 또한 세션을 감지하는 작업을 수행하기도 합니다.

이런 여러 동작들 사이에 끼워져서 실행되는 서블릿 클래스를 **필터**라 부릅니다. 필터는 말 그대로 **여과 기능**을 수행합니다.

필터는 Filter Interface 를 implement 하여 표준에 따라 구현해주면 사용할 수 있습니다. 이러한 필터가 웹 프로그램에 사용되는 예시는 다음과 같습니다.

- 전달받은 데이터를 인코딩하는 경우
- 세션 데이터를 인증하는 경우
- 이벤트나 공지 등 팝업을 추가하는 경우

필터는 **web.xml**에서 설정할 수 있으나 오타, 가독성 등의 문제가 발생할 수 있습니다. 현재는 필터 클래스 위 `@WebFilter()` **Annotation**을 활용한 방법이 대부분입니다.

어노테이션 인자로 넘겨지는 **url-pattern**에 따라 필터가 동작합니다. 요청된 URL을 확인하여 이것과 부합하는 필터가 있을 시, 필터 클래스가 실행됩니다.