---
title: "[Spring] Layered Architecture & MVC 패턴"
excerpt: "layered architecture"
date: 2022-02-12
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - layered architecture
    - component
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Layered Architecture

효율적인 개발과 유지보수를 위해 계층화하여 개발하는 구조를 계층화 아키텍처(Layered Architecture)라고 한다. 대부분의 중/대규모 어플리케이션에 적용되는 구조로 각 레이어는 독립된 R&R을 가진다. 근접한 레이어들끼리만 데이터를 주고 받을 수 있으며, 하나의 층을 건너뛰어 직접적으로 연결되는 것을 금지한다.

각 레이어는 프레젠테이션 영역, 비즈니스 영역, 데이터 영역 세 가지로 일반적으로 구분된다.

<br/>
<span style="font-size: 1.5em">프레젠테이션 영역</span>
<br/>

- 사용자와 상호작용 담당
- 사용자의 요청을 분석
- 사용자 요청에 대한 응답

<br/>
<span style="font-size: 1.5em">비즈니스 영역</span>
<br/>

- 기능을 수행
- 트랜잭션 수행

<br/>
<span style="font-size: 1.5em">데이터 영역</span>
<br/>

- 데이터 저장과 조회를 담당
- 데이터베이스와 연동하여 작업

<br/>

보통의 어플리케이션은 화면과 데이터베이스를 양 끝단으로 두고 있다. 양 끝에 Presentation Layer 와 Data Access Layer 가 붙어있으며 그 사이에 Business Logic Layer 가 존재한다. 비즈니스 로직 레이어에서는 기능에만 집중할 수 있도록, 서비스 구현에만 집중할 수 있도록 데이터 영역이 분리된 것이 핵심처럼 느껴진다.

## MVC 패턴

Layered Architecture 를 사용한 대표적인 패턴이다. 어플리케이션을 Model, View, Controller 세 가지로 구분한다. UI를 가지는 대부분의 어플리케이션은 MVC 혹은 변형된 MVC 패턴을 따른다. 이번에 한 가지 배운 점은 **MVC 패턴은 Presentation Layer 를 조금 더 세분화 한 것** 이라는 점.

MVC 패턴을 통해 높아지는 화면 요구사항과 유지/보수 용이성 두 가지를 모두 만족할 수 있다고 한다. 아래는 MVC 패턴의 세 가지 요소가 가진 속성이다.

- Model : 데이터 저장, 처리
- controller : User Interface, 화면
- View : 사용자 요청(입력) 처리, 모델과 화면을 서로 연결

MVC 패턴에 대해 조금 더 자세히 알아보자. MVC(Model-View-Controller)는 소프트웨어 공학에서 사용되는 아키텍처 패턴으로 MVC 패턴의 주 목적은 Business Logic 과 Presentation Logic 을 분리하는 것이다.

MVC 패턴을 사용하면, 앞서 말했던 것 처럼 사용자 인터페이스로부터 비즈니스 로직을 분리해 어플리케이션의 시각적 요소나 이면에서 실행되는 비즈니스 로직을 서로 영향 없이 쉽게 고칠 수 있다. 한 개의 코드에 장황하게 쓰여진 1,000줄과 기능별로 나뉘어진 200줄 짜리 코드 5개.. 코드가 복잡해질 수록 후자가 유지, 관리, 보수의 편리성을 갖춰나갈 것은 분명한 얘기.

각 MVC 컴포넌트의 역할을 간략히 살펴보면 아래와 같이 나타낼 수 있다.

- 모델(Model) 컴포넌트
  - 데이터 저장소(ex : 데이터베이스 등)와 연동하여 사용자가 입력한 데이터나 사용자에게 출력할 데이터를 다루는 일 함
  - 여러 개의 데이터 변경 작업(추가, 변경, 삭제)을 하나의 작업으로 묶는 트랜잭션을 다룸
  - DAO 클래스, Service 클래스와 연결 (또는 Business Layer 와 연결)

- 뷰(View) 컴포넌트
  - 모델이 처리한 데이터나 그 작업 결과를 가지고 사용자에게 출력할 화면을 만드는 일 수행
  - 뷰 컴포넌트는 HTML, CSS, Javascript 로 작성된 문서를 생성
  - HTML과 JSP를 비롯해 여러 프레임워크가 사용될 수 있음

- 컨트롤러(Controller) 컴포넌트
  - 클라이언트 요청에 대해 실제 업무를 수행하는 모델 컴포넌트를 호출하는 일 수행
  - 클라이언트가 보낸 데이터를 가공함
  - 모델의 업무 수행이 완료되면 해당 결과를 뷰에 전달해 화면 생성 요청
  - Servlet과 JSP를 비롯한 다양한 Server-side 프레임워크가 사용될 수 있음

Spring은 MVC 패턴에서 Front Controller 패턴 아키텍처를 지원한다. 요청에 따른 컨트롤러의 동작이 점차 다양해졌기 때문이라고 한다.

Front Controller는 클라이언트가 보낸 요청을 받아 공통적인 작업을 먼저 수행한다. 그리고 인증이나 권한 체크와 같은 공통 작업을 수행한 후 세부 Controller 에게 작업을 위임한다. 이전에는 이런 작업을 Filter 를 직접 작성해야 했다. 다행히 Spring에는 내장된 프론트 컨트롤러를 지원한다.

방법은 간단히 Front Controller 역할을 하는 DispatcherServlet 이라는 클래스를 계층의 맨 앞단에 놓고 서버로 들어오는 모든 요청을 받아 처리하는 형태로 구현된다. Spring이 제공하는 MVC 패턴인 Spring MVC 의 주요 구성 요소를 아래와 같이 정리할 수 있다.

- DispatcherServlet : 클라이언트 요청을 받아 Controller 에게 클라이언트 요청을 전달하고, 반환된 결과값을 View 에게 전달해 화면 생성 요청
- HandlerMapping : URL과 요청 정보를 기준으로 어떤 핸들러 객체를 사용할지 결정하는 객체이며, DispatcherServlet은 하나 이상의 핸들러 매핑을 가질 수 있음
- Controller : 클라이언트 요청을 처리한 뒤, Model을 호출하고 결과를 DispacherServlet 에게 전달
- ModelAndView : Controller가 처리한 데이터 및 화면에 대한 정보를 보유한 객체
- View : Controller 처리 결과 화면에 대한 정보를 보유한 객체
- ViewResolver : Controller 가 리턴한 뷰 이름을 기반으로 Controller 처리 결과를 생성할 뷰를 결정

## Spring MVC 설정

Spring MVC 설정은 아래와 같이 진행된다.

- spring-mvc 라이브러리 설정 (pom.xml)
- DispatcherServlet 설정 (web.xml)
- 스프링 빈 설정 파일 (WEB-INF/spring/spring-context.xml)
  - `<context:component-scan .. >`
  - `<mvc:annotation-driven />`
  - viewResolver 설정

WEB-INF/web.xml 파일도 아래와 같이 간단하게 설정하면 된다. 먼저 dispatcherServlet 이름으로 스프링 프레임워크에서 지원하는 DispatcherServlet 클래스를 바인딩한다. 해당 서블릿 설정 정보가 담긴 위치를 그 아래 파라미터로 전달한다. 끝으로 전달되는 URL에 대해 어떤 서블릿으로 전달할 것인가를 매핑한다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">

  <servlet>
  	<servlet-name>dispatcherServlet</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<init-param>
  		<param-name>contextConfigLoaction</param-name>
  		<param-value>/WEB-INF/spring/servlet-context.xml</param-value>
  	</init-param>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>dispatcherServlet</servlet-name>
  	<url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>`
```

다음으로는 앞에서 지정한 servlet-context.xml 세부 내용을 채운다.

viewResolver 는 설정 방법이 생소하지 않다. 컨트롤러는 모델에서 반환받은 데이터 정보를 View 로 전달한다. 이 때, 동적 페이지 생성을 위한 jsp 파일들을 특정해야 하는데 이름 만으로 식별할 수 있도록 `prefix` 와 `suffix` 를 지정한다.

```xml
<!-- servlet-context.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.3.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">

	<context:component-scan base-package="com.example.hello"></context:component-scan>
	<mvc:annotation-driven></mvc:annotation-driven>
	
	<!-- jsp 파일의 이름만 적으면 /WEB-INF/views/ 내부에서 이름만 가지고 jsp 파일 요청 -->
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/views/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
</beans>
```

# Layered Architecture 기반 개발

Spring 에서는 어노테이션(Annotation)을 사용한 Layered Architecture 구현을 지원한다. `@Component` 와 하위 `@Controller`, `@Service`, `@Repository` 어노테이션을 사용하면 MVC 패턴을 구현할 수 있다. 각각은 프레젠테이션, 서비스, DAO를 구현하는데 사용할 수 있다.

코딩을 할 때, 내가 지금 건드리는 부분이 Layered Architecture 의 어디에 속하는지 생각하면서, 흐름을 생각하면서 짜면 이해가 빨라진다. 예를 들어, DAO를 개발한다는 것은 Data Access Layer 부분에 접하는 곳을 건드리는 중이란 얘기. 인접한 Layer 에서만 사용할 수 있기 때문에 영향도를 파악하기 위해서는 자연스레 Business Layer 를 살펴보면 된다. 즉, Service 라고 이름붙은 코드를 살펴보자.

소스코드를 많이 보다보니 자연스럽게 터득한 것인데, 이렇게 글로 정리해보니 다시 새롭게 느껴진다. 비슷한 예는 Presentation Layer 에도 있다. VO를 받아 DAO로 전달하는 서비스가 그 예시다. 사용자가 웹에서 어떤 요청을 하면 그 정보들을 모두 모아 하나의 오브젝트 형태로 만들고, 이걸 서비스에 전달한다. 서비스는 이것저것 역할을 수행하고, 필요하면 그 오브젝트를 DAO에 전달해 데이터베이스에 대한 동작을 수행한다. 물론 이 동작은 단순 삽입이 아니라 CRUD 하겠지.

참고로 VO(Value Object)는 DTO(Data Transfer Object) 와 비슷한 개념으로 둘의 차이는 Getter 만 있느냐 Getter & Setter 모두 있느냐, 로 생각하면 되겠다. VO는 Getter 만 있어서 한 번 생성자로 값이 삽입되면 바꿀 수 없는 Immutable 한 성질을 가지고 있다.

그니까.. 머리에 이 그림을 박고.. 아니지. 개발리더가 어떤 그림을 그려서 "이게 우리 구조야" 라고 하면 그 구조를 기억하면서 개발하면 된다. 훨씬 편하게, 정확하고, 구조적으로 개발할 수 있을 것이다. 아래는 그 유명한 3-tier 구조도다.

<br/>
<img src="/_img/2022-02-12/2010s-layered-architecture.png" href="https://herbertograca.com/2017/08/03/layered-architecture/">
<br/>

## 예제

아래는 게시판의 글쓰기 기능을 구현한 예제다. 아래 4가지를 키 포인트로 한다.

- 게시판에 글을 쓰는 `BbsService.registArtice(Artice)`과 게시글 DAO `ArticleDAO.insertArtice(Artice)` 메소드를 구현한다.
- Service, DAO, VO 세 가지를 통해 Layering 한다.
- `@Service`, `Repository` 어노테이션을 `<context:component-scan .../>` 을 사용해 자동으로 컨테이너에 등록되도록 한다. 
- `@Autowired`를 이용해 자동으로 생성한 빈을 주입하도록 만들 것이다.

먼저 STS(Spring Tool Suite) 설치할 때, 꼭 Add-on 같이 설치하기 잊지 말자. XML에 Namespace 탭 없는 것 보고 부랴부랴 찾다가 알았다.

DTO 먼저 정의해보자. Getter 와 Setter 모두 만들었다.

```java
package com.example.hello.vo;

public class Article {
	private int articleId;
	private String authorString;
	private String titleString;
	private String content;
	
	public Article() {
		
	}

	public Article(int articleId, String authorString, String titleString, String content) {
		super();
		this.articleId = articleId;
		this.authorString = authorString;
		this.titleString = titleString;
		this.content = content;
	}



	public int getArticleId() {
		return articleId;
	}

	public void setArticleId(int articleId) {
		this.articleId = articleId;
	}

	public String getAuthorString() {
		return authorString;
	}

	public void setAuthorString(String authorString) {
		this.authorString = authorString;
	}

	public String getTitleString() {
		return titleString;
	}

	public void setTitleString(String titleString) {
		this.titleString = titleString;
	}

	public String getContent() {
		return content;
	}

	public void setContent(String content) {
		this.content = content;
	}

	@Override
	public String toString() {
		return "Article [articleId=" + articleId + ", authorString=" + authorString + ", titleString=" + titleString
				+ ", content=" + content + "]";
	}	
}
```

이렇게 만든 데이터는 MVC 중 모델을 통해 비즈니스 로직으로 전개된다. Article 을 받아서 DAO 에게 전달하는 서비스를 만들면 아래와 같이 나타낼 수 있다.

```java
package com.example.hello.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.example.hello.dao.ArticleDAO;
import com.example.hello.vo.Article;

@Service
public class BbsService {
	@Autowired
	private ArticleDAO articleDAO;
	
	public void setArticleDAO(ArticleDAO dao) {
		this.articleDAO = dao;
	}
	
	public void registerArticle(Article article) {
		articleDAO.insertArticle(article);
	}
}
```

끝으로 여기서 사용되는 DAO를 간단하게 만들어 볼 수 있다. 각 메소드에는 실제 DB와 연동되는 쿼리가 사용될 수 있지만 간단한 예제니까..

```java
package com.example.hello.dao;

import org.springframework.stereotype.Repository;

import com.example.hello.vo.Article;

@Repository
public class ArticleDAO {
	public void insertArticle(Article article) {
		System.out.println("insert ok...");
	}
}
```

각 계층별로 만든 자바 소스들에 어노테이션이 붙은 것들을 볼 수 있다. 이들을 컨테이너에서 관리하기 위해 컴포넌트 스캔을 마지막으로 삽입한다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">
		
		<context:component-scan base-package="com.example.hello.service"></context:component-scan>
		<context:component-scan base-package="com.example.hello.dao"></context:component-scan>
		
		<!-- @Service, @Repository 없을 때, 수동으로 등록하는 부분 
		<bean id="articleDAO" class="com.example.hello.dao.ArticleDAO"></bean>
		<bean id="bbsService" class="com.example.hello.service.BbsService">
			<property name="articleDAO" ref="articleDAO"></property>
		</bean>
		 -->
</beans>
```