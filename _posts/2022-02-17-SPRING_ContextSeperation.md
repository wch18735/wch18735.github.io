---
title: "[Spring] Context 분리 전략"
excerpt: "context seperation strategy"
date: 2022-02-17
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - aop
    - aspectj
    - jdbc
author: 1FeS
comments: true
---

# Servlet context vs Root Application context

Servlet Context

- DispatcherServlet : 웹 요청을 최초로 접수
- 설정 파일을 이용해서 ServletContext (Spring container) 로딩
- Spring-MVC와 관련된 빈(Bean)을 설정
- 각각의 Servlet Context 에 로딩된 빈(Bean)은 서로를 참조할 수 없다
- Web 과 관련된 빈을 관리하며 여러 개 생성될 수 있다

Root Context

- Spring-MVC와 분리되어 빈(Bean)을 관리하고 싶을 때 사용
- 일반적으로 소스코드 크기가 커질 때, Servlet context와 Root context 분리하여 운용
- Servlet context 에서 공통적으로 접근할 수 있는 Context
- Web 과 관련되지 않은 서비스, DAO 등을 다루며 하나만 생성한다

소스코드 사이즈가 커질 수록 위와 같이 컨텍스트들을 분리한다. 아래는 Spring-MVC 의 Context 구조를 보여준다.

<br/>
<img src="/_img/2022-02-17/spring mvc context hirachy structure.png" style='margin: auto auto;'><br/>

먼저 Root Application Context 를 알아보자. Root Application Context 는 web.xml 에 Listener 클래스를 하나 지정해 생성해준다. 물론 여러 개의 설정 파일을 지정할 수 있다. 이에 대한 간략한 정리를 하면 아래와 같다.

- 전체 계층구조에서 최상단에 위치한 컨텍스트
- 서로 다른 서블릿 컨텍스트에서 공유해야하는 Bean 들을 등록해놓고 사용
- 웹 어플리케이션 전체에 적용 가능한 DB 연결, Logging 등에 이용
- Servlet Context 에 등록된 Bean 을 참조할 수 없는 단방향 관계
- Servlet Context 와 동일한 Bean 이 있을 경우 Servlet Context 내부의 Bean 이 우선 순위
- 하나의 컨텍스트에 정의된 AOP 설정은 다른 컨텍스트 빈에는 영향을 미치지 않음

다음으로 Servlet Context 를 정리한다. 서버에서 사용하는 조각과 같은 의미로 Serv + let 이라고 한다는 얘기를 들었다. 보통 Java 소스 코드로 이뤄져 백엔드에서 동작을 정의한 코드라고 생각해보자. 컨텍스트는 이런 Java 소스가 컴파일되어 메모리에 올라간 공간이라고 이해하자.

- 특정한 Servlet 들만 관리하는 컨텍스트
- 타 서블릿과 공유하기 위한 Bean 들은 Root Web Application Context 에 등록 필요
- DispatcherServlet 은 자신만의 컨텍스트를 생성하며 초기화와 동시에 Root Application Context 를 찾아 부모 컨텍스트로 사용

## 컨텍스트 분리 전략

Servlet Context 는 `indclude-filter` 를 사용해 아래와 같이 `@Controller` 만을 포함하도록 필터를 적용한다.

```xml
<context:component-scan base-package="com.example" use-default="false">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

반대로 Root Context 는 `exclude-filter` 를 사용해 `@Service` 와 `@Repository` 어노테이션으로 등록된 객체만을 포함하도록 한다. 이것이 `@Component` 어노테이션이 세 가지로 나뉘는 이유라고 생각된다.

```xml
<context:component-scan base-package="com.example" use-default="false">
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

## 컨텍스트 분리 실습 Tip

- spring bean configuration xml 로 파일 추가
- Window > Perspective > Open perspective > Other.. > Spring 경로로 Spring Explorer 를 열어 생성된 빈들을 확인할 수 있음
- database.properties 에서 `db.driverClass=oracle.jdbc.driver.OracleDriver` 로 oracle 은 형태가 조금 다름을 주의