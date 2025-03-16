---
title: "[Spring] Spring-Data Access 기술과 DataSource"
excerpt: "spring data source"
date: 2022-02-16
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

# DAO 디자인 패턴

DAO 디자인 패턴은 비즈니스 서비스와 로우레벨 데이터 엑세스 작업을 분리하는 구조적 설계 형상이다. DAO 레이어는 데이터베이스와 비즈니스 레이어 가운데 위치해 CRUD 작업을 수행한다. DAO 에서는 데이터베이스에 접근하는 Data Access 가 이뤄진다.

## 데이터 엑세스 기술

데이터 엑세스 기술로는 막연하게 알고있으나 개념조차 흐릿했던 내용들을 간단하게 먼저 정리해보려고 한다. 

<br/>
<span style="font-size:1.2em">JDBC</span>
<br/>

먼저 JDBC다. JDBC는 자바에서의 관계형 데이터베이스 접속 기술이다. 즉, 데이터베이스에 연결할 수 있도록 구현된 인터페이스이다. JDK 레벨에서 지원하며, 다른 고수준 기술의 기반이 된다. TMI로 Low-code platform 인 Mendix 역시 데이터베이스와 통신할 때, JDBC를 사용한다. 

<br/>
<span style="font-size:1.2em">Spring JDBC</span>
<br/>

Spring JDBC는 스프링 자체적으로 지원하는 JDBC Wrapper 이다. 

- 스프링 프레임워크에 포함된 공식 데이터 베이스 연동 라이브러리
- 유연한 Transaction 처리를 위해 Connection 단의 높은 추상화 제공
- Connection-Pool 지원하고, Result Mapper 추상화를 사용해 쿼리를 편하게 수행할 수 있음
- MyBatis, Hibernate 등이 Spring-JDBC를 사용

<br/>
<span style="font-size:1.2em">MyBatis</span>
<br/>

MyBatis 는 Third-party 자바-관계형DB 매핑 라이브러리로 자바 오브젝트와 SQL문을 자동으로 매핑해주는 SQL Mapper 를 지원해준다. 국내에서는 MyBatis가 굉장히 많이 사용된다. 

JDBC 프로그래밍을 위해 String 타입의 SQL문이 소스코드 상에 개발 초반부터 삽입되는 형태는 개발자들에게 관리 부다의 원인이 되었다고 한다. 이 문제를 MyBatis로 해결할 수 있었다. 

- XML을 별도 파일로 분리하고 자바 객체와 SQL 매핑을 지원하는 프레임 워크
- 별도 라이브러리로 존재하고 스프링과 연동 모듈을 지원
    - mybatis-spring 라이브러리 필요
    - 스칼라 MyBatis, .NET MyBatis 등이 있음

<br/>
<span style="font-size:1.2em">JPA</span>
<br/>

Java ORM 표준 인터페이스이다. ORM이란 Object Relation Mapping 기술로 객체와 관계형 데이터베이스를 연결하는 고도로 추상화된 연결 인터페이스이다. 

<br/>
<span style="font-size:1.2em">Hibernate</span>
<br/>

JPA 라는 자바 ORM을 구현하는 구현체이다. 기존의 JDBC 기반 프로그래밍과는 사뭇 다른 데이터베이스 엑세스 패러다임을 가진다. 객체 자체가 데이터베이스와 연동된다는 개념인데.. 아직 감이 잡히진 않는다.

자바 객체를 생성하는 것으로 INSERT가 동작되며, 객체를 수정 후 관계를 정의해주기만 하면 자동으로 데이터베이스 테이블에 반영이 된다. 그렇기 때문에 이러한 기술은 SI 에서 보다는 솔루션에 많이 사용된다고는 하지만.. 점차 SI / 운영에 적용될 여지가 충분하다고 생각한다. 얼른 배워둬서 나중에 꼭 써먹어보고 싶다.

<br/>
<span style="font-size:1.2em">DataSource</span>
<br/>

외부 데이터베이스를 동작시키기 위해서는 연결 객체를 얻어내는 것이 가장 중요하다. 어떤 데이터 접근 기술을 사용하더라도 연결 객체를 얻는 과정은 필수로 진행된다. 그렇기 때문에 Connection 을 제공해주는 DataSource 는 데이터 접근 기술의 핵심 요소라고 할 수 있다.

- Database 에 연결하는 Connection 확보
- 데이터소스는 Connection 을 가져오기 위한 기술
- 스프링은 DataSource 를 통해 Connection 제공

<br/>
<span style="font-size:1.2em">DataSource 설정 방법</span>
<br/>

- DriverManager 사용 방법
    - DriverManagerDataSource 클래스 사용
    - Connection Pool 이나 JNDI 를 사용할 수 없는 경우에 사용
- Connection Pool 사용 방법
    - 데이터베이스 커넥션을 미리 여러 개 확보해두고 클라이언트의 요청에 따라 제공하고 반납받음
    - 매번 물리적으로 커넥션을 생성하지 않아 성능 향상
    - 운영상에서는 Connection Pool 을 사용하는 것이 바람직함
    - Tomcat 에서도 Connection Pool 을 지원함

## 프로퍼티(Property) 파일을 이용한 설정

물리적 데이터베이스와 연결한다는 뜻은 드라이버 이름, 접속 URL, 사용자 이름, 비밀번호 등을 어딘가에는 사용한다는 뜻이다. 개발과 운영의 데이터베이스가 달라질 수 있으며, 데이터베이스 접속 환경 역시 달라질 수 있기 때문에 Property 파일 관리 역시 중요한 영역이다.

일반적인 경우, 환경에 따라 자주 변경되는 내용을 분리한다고 한다. 단순한 Key, Value 형태 쌍으로 구성해 관리하는 방법이 변경, 관리에 유리하다. 아래와 같이 `context:property-placeholder` 를 통해 프로퍼티 파일이 있는 곳을 잡아준다. 이 때, classpath root 는 resources 폴더다.

```xml
<!-- DataSource : Bean 형태 -->
<context:property-placeholder location="classpath:config/database.properties"/>
<bean id="dataSource" class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
    <property name="driverClass" value="${db.driverClass}"></property>
    <property name="url" value="${db.url}"></property>
    <property name="username" value="${db.username}"></property>
    <property name="password" value="${db.password}"></property>
</bean>
```

위 코드에서 `${}` 접근자를 통해 프로퍼티 파일에 접근을 수행한다. 프로퍼티 파일 내부는 아래와 같이 작성해주면 된다. 따옴표를 사용하지 않는 것을 주의하자.

```sh
db.driverClass=com.oracle.jdbc.Driver
db.url=jdbc:oracle:thin:@//localhost:1521/xepdb1
db.username=USERNAMe
db.password=PASSWORD
```