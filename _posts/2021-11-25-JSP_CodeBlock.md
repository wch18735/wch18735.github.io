---
title: "[JSP] JSP 코드 블록"
excerpt: "jsp code block using =, !, @"
date: 2021-11-25
layout: single
classes: wide
category:
    - jsp
tag:
    - java
    - jsp
    - servlet
    - code block
toc: true
toc_label: "Contents"
author: 1FeS
comments: true
---

# JSP 프로그래밍이란?

*JSP(JavaServerPage)* 는 HTML 코드에 Java 코드를 넣어 동적인 페이지를 생성하는 것이다. 여러 마디 말 보다 한 가지 예가 더 직관적으로 이해를 도울 때가 있다.

```java
// .jsp file
<% for(int i = 0; i < 10;i++) { %>
    <tr>
        <td><%= i+1 %></td>
        <td>하나씩 늘어나는 값</td>
    </tr>
<% } %>
```

`<% %>` 로 감싸지는 영역 내부는 모두 자바 코드로 인식된다. `<% %>` 내부는 변수 사용이 가능하기 때문에 위 코드는 `for문` 안에서 테이블 10개를 그리게 된다.

이런 코드 블록은 아래 네 가지가 존재한다.

- `<%  %>`: 한 줄에 자바 코드 사용할 수 있으나 줄바꿈 불가
- `<%= %>`: out.print() 없이 그대로 변수 출력 가능
- `<%! %>`: 여러 줄에 걸쳐 자바 코드 작성 가능
- `<%@ %>`: 다른 설정자들보다 가장 먼저 동작함

가장 마지막 코드블럭의 예시로 아래와 같은 *jsp 파일 지시자* 가 사용된다. 데이터베이스와 연동하기 위한 설정을 미리 수행하기 위해 `.jsp` 상단에 명시한 내용이다.

```java
<%@page import="java.sql.DriverManager"%>
<%@page import="java.sql.ResultSet"%>
<%@page import="java.sql.Statement"%>
<%@page import="java.sql.Connection"%>
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>

<%
String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
String sql = "SELECT * FROM NOTICE";

Class.forName("oracle.jdbc.driver.OracleDriver");
Connection con = DriverManager.getConnection(url, "id", "password");
Statement st = con.createStatement();
ResultSet rs = st.executeQuery(sql);
%>

/*
...
... html code contents
...
*/

<% 
rs.close();
st.close();
con.close();
%>
```

*Jasper* 는 이렇게 만든 HTML 코드를 서블릿으로 변환해주는 엔진이다. 

`.html` 확장자 대신 `.jsp` 로 작성된 문서를 이 *Jasper* 가 해독해 명령에 맞는 동적 웹페이지를 생성한다. 즉, 여러 개의 `out.write()` 로 직접 표현했어야 할 웹 문서를 *Jasper* 가 대신 만들어주는 셈.

참고로 위 코드를 삽입해 처음 데이터베이스 연결 설정을 시도하면 *HTTP 상태 500* 오류가 발생한다. *ORACLE JDBC 드라이버* 가 없기 때문이다.

WEB-INF 내부 LIB 폴더에 필요한 라이브러리 파일을 복사해 붙여 넣어 문제를 해결할 수 있다. 실제로 웹 어플리케이션 배포 시 해당 폴더에 외부 라이브러리들을 담아 빌드한다고 얘기한다.

