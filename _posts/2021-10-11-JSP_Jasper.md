---
title: "[JSP] Jasper 활용 예제"
excerpt: "JSP, Jasper를 이용한 서블릿 프로그래밍"
date: 2021-10-11
layout: single
classes: wide
category:
    - jsp
tag:
    - java
    - jsp
    - servlet
    - jasper
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Jasper를 이용한 웹 개발

<img src="/_img/2021-10-10/documents.jpg">

이번 강의로 배운 것. 보통 왼쪽의 파일들이 홈 디렉토리라고 생각하지만, 톰캣이 배포하는 디렉토리는 다른 곳이 있습니다. 이를 서버의 디렉토리라고 합니다.

이는 `.metadata`의 `org.eclipse.wst.server.core` 에서 확인할 수 있습니다. 이곳을 살펴보면 개발 디렉토리와 닮은 `tomcat` 디렉토리가 있는 것을 볼 수 있습니다.

tmp 안의 work 디렉토리를 확인하면 **Jasper**가 동작하는 곳을 찾을 수 있습니다. 이곳에서 컴파일되는 `.jsp` 파일들과 클래스 파일을 확인할 수 있습니다.

## Codeblock (코드 블록)

Jasper에게 이런 내용을 전달합니다. <span style="font-weight: bold; color: red">이건 코드야! 그냥 출력하면 안 돼!</span> 라는 명령을 Jasper 에게 알려줄 때, 우리는 **코드 블럭**을 사용합니다.

```jsp
<%
int x = 3;
int y = 4;
%>

...
html contents
...
```

이렇게 변수를 전달하고, 변수들을 의도한 대로 동작시키는 서블릿을 개발하는 것이 **서블릿 개발**입니다. 이렇게 사용되는 코드 블럭은 다양한 예시가 있습니다.

먼저 변수를 출력 문자열 사이에 삽입하는 예제입니다.

```jsp
y의 값은 <%out.print(y)%>
```

이 과정이 너무 어렵다면 다음과 같이 줄일 수 있습니다.

```jsp
y의 값은 <%=y%>
```

또한 우리는 코드 블럭을 이용해 멤버 메서드를 정의하는 방법도 있습니다. 그러나 코드 블럭을 작성하면 일반적으로 **지역 변수**에 포함됩니다. 그렇기 때문에 앞에 `!`를 붙여 멤버함수 멤버변수로 삽입할 수 있습니다.

```jsp
<%!
    public int sum(int a, int b) {return a + b;}
%>
```

마지막으로 초기 설정을 위한 페이지 지시자 **(@)** 를 들 수 있습니다. 이렇게 표기되는 설정들은 어떠한 출력보다 가장 먼저 지시되어 설정됩니다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
```

## JSP 내장객체

코드 블럭 안에 `int page=1`을 삽입하면 오류가 나타납니다. 바로 중복된 변수명이 있다는 것입니다. Jasper는 사실 페이지를 생성하기 위해 몇 가지 변수들을 미리 설정해 놓았습니다. 해당 변수들과 충돌하는 변수명은 오류를 발생시킵니다.

이렇게 만들어 놓은 객체들을 내장형 객체라고 합니다. 대표적인 내장형 객체들은 아래와 같습니다.


- request
- response
- pageContext
- session
- application
- config
- out
- page

정리하면 **내장객체란 Jasper가 만들어낸 서블릿 안에 있는 객체**라고 표현할 수 있습니다..

이들을 잘 알아두고, 웹 개발 시 해당 객체들을 이용해 기능 구현하는데 이용할 수 있도록 합니다. 추후 **잘 정리된 jsp 템플릿**을 이용하며 자주 마주치게 될 문제 중 하나를 살펴보았습니다.