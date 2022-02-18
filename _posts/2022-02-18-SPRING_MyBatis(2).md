---
title: "[Spring] MyBatis 소개"
excerpt: "what is mybatis"
date: 2022-02-18
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

# MyBatis 개요

MyBatis는 자바 오브젝트와 SQL문 사이의 자동 Mapping 기능을 지원하는 Object Mapper 이다. 기존 JDBC를 사용한 Database Access 기법의 불편한 점이 대폭 개선된 기술이다. 물론 ORM 기술과는 다르며 JPA와 JPA 구현체인 Hibernate 와는 사용 방법이 다르다고 할 수 있다.

MyBatis는 SQL을 별도의 파일로 분리해서 관리하게 해주며, 객체-SQL 사이의 파라미터 Mapping 작업을 자동으로 해주기 때문에 많이 사용된다. MyBatis는 Hibernate 나 JPA(Java Persistence API)처럼 새로운 DB 프로그래밍 패러다임을 익혀야하는 부담없이 개발자가 익숙한 SQL을 그대로 이용하면서 JDBC 코드 작성의 불편함도 제거해주고, 도메인 객체나 VO 객체를 중심으로 개발이 가능하다는 장점도 있다.

# MyBatis 특징

MyBatis 특징에 대해서 간단히 정리해보려 한다.

## 쉬운 접근성과 코드의 간결함

- 간단한 퍼시턴스 프레임워크
- XML 형태로 서술된 JDBC 코드라고 생각해도 될 만큼 JDBC 모든 기능을 MyBatis 가 대부분 제공함
- 선언적인 방법으로 간단하게 코드를 관리할 수 있는 기능을 제공
- 복잡한 JDBC 코드를 걷어내며 깔끔한 소스코드를 유지할 수 있음
- 수동적 파라미터 설정과 쿼리 결과에 대한 맵핑 구문을 제거할 수 있음

## SQL문과 프로그래밍 코드 분리

- SQL 변경이 있을 때마다 자바 코드를 수정하거나 컴파일하지 않아도 된다
- SQL 작성과 관리 또는 검토를 DBA와 같은 개발자가 아닌 다른 사람에게 맡길 수 있음

## 다양한 프로그래밍 언어로 구현 가능

- `Java`, `C#`, `.NET`, `Ruby`

# MyBatis Database Access

아래 그림은 MyBatis3, MyBatis-Spring 의 데이터베이스 접근 과정을 모식도로 나타낸 것이다. 기존 JDBC Interface 만을 사용하던 단계에서 `@Repository` 빈(Bean)과 O/R Mapper 가 추가된 것에 주목하자.

<br/>
<img src="/_img/2022-02-18/mybatis_mybatis-spring_data_access.png" style="margin: auto auto;">
<br/>

MyBatis3의 주요 컴포넌트를 검색해보면 아래 그림을 굉장히 많이 볼 수 있다. 보라색으로 표신된 부분은 개발자가 개발하는 부분이며 파란 부분은 MyBatis3에서 제공되는 것이다.

<br/>
<img src="/_img/2022-02-18/mybatis3_component.png">
<br/>

먼저 데이터베이스 작업에 대한 요청이 들어온다. 해당 작업을 수행하기 위해서는 SqlSession 이라는 것이 필요하다. MyBatis에서는 SqlSession 이 내부적으로는 Connection 을 가지고 있다. Connection 은 이전에도 말했듯 데이터베이스 작업을 위해서 반드시 필요한 것이다.

SqlSession 을 받아오기 위해서 몇 가지 단계를 거친다. 먼저 SqlSessionFactoryBuilder 라는 클래스를 호출한다. SqlSessionFactoryBuilder 는 MyBatis Config File 에 작성되어 있고, 이를 참조해 요청에 대한 SqlSessionFactory 를 제공한다. SqlSessionFactory 는 추후에 SqlSession 을 만들어주기 위한 Factory 이다. 

`(6)` 과정이 SqlSessionFactory 에서 SqlSession 을 받아오는 작업을 나타낸다. 해당 부분들은 모두 MyBatis 에서 제공되는 것으로 XML에 기재된 설정 파일에 의해 동작한다.

위에서 다룬 MyBatis3의 주요 컴포넌트 역할을 정리해보면 아래와 같다.

- MyBatis 설정파일 (SqlMapConfig.xml): 데이터베이스 접속 주소 정보나 Mapping 파일 경로 등 고정된 환경 정보를 설정
- SqlSessionFactoryBuilder: MyBatis 설정 파일을 바탕으로 SqlSessionFactory 를 생성
- SqlSessionFactory: SqlSession 을 생성
- SqlSession: 핵심적인 역할을 하는 클래스로 SQL 실행, 트랜잭션 관리 수행. SqlSession 오브젝트는 Thread-safe 하지 않으므로 thread 마다 필요에 따라 생성.
- mapping 파일 (user.xml): SQL문과 OR Mapping 을 설정

다음은 MyBatis-Spring 주요 컴포넌트에 관한 내용이다. SqlSessionTemplate 은 MyBatis의 Thread-unsafe 한 SqlSession 오브젝트를 구현해놓은 것으로 Thread-safe 한 성질을 가진다.

<br/>
<img src="/_img/2022-02-18/mybatis-spring_component.png">
<br/>

MyBatis-Spring 의 주요 컴포넌트 역할을 정리하면 아래와 같다. 이와 관련된 간단한 예제는 [이곳](https://wch18735.github.io/spring/SPRING_MyBatisExample)에서 확인할 수 있다.

- MyBatis 설정 파일 (sqlMapConfig.xml): VO 객체 정보 설정
- SqlSessionFactoryBean: MyBatis 설정파일을 바탕으로 SqlSessionFactory 생성. 이를 사용하기 위해 Spring Bean 등록이 필요함.
- SqlSessionTemplate: 핵심적 역할을 하는 클래스로 SQL 실행이나 트랜잭션 관리 수행. SqlSession 인터페이스를 구현하며, Thread-safe 하다. 이를 사용하기 위해선 Spring Bean 으로 등록해야 한다.
- Mapping 파일 (user.xml): SQL문과 OR Mapping 설정
- Spring Bean 설정파일 (beans.xml): SqlSessionFactoryBean을 등록할 때 DataSource 정보와 MyBatis Config 파일 정보, Mapping 파일 정보를 함께 설정한다. 또한 SqlSessionTemplate을 Bean으로 등록한다.

이를 정리하면 아래와 같이 나타낼 수 있다.

<br/>
<img src="/_img/2022-02-18/mybatis_mybattis-spring_component.png" style="margin: auto auto;">
<br/>

## 예제 코드

MyBatis 홈페이지의 시작하기로 들어가보면 MyBatis Main 설정 파일의 예제가 나온다. 이를 그대로 복사한 후 resources 디렉토리에 설정 파일을 만들어준다.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

이후 datasource.xml 파일(데이터베이스 서블릿 컨텍스트 설정 담당)에 mybatis 설정을 수행한다. mybatis 역시 datasource 를 사용하기 때문이다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:lang="http://www.springframework.org/schema/lang"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:jdbc="http://www.springframework.org/schema/jdbc"
	xsi:schemaLocation="http://www.springframework.org/schema/jdbc http://www.springframework.org/schema/jdbc/spring-jdbc-4.3.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/lang http://www.springframework.org/schema/lang/spring-lang-4.3.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">

	<!-- DataSource : Bean 형태 -->
	<context:property-placeholder
		location="classpath:config/database.properties" />
	<bean id="dataSource"
		class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
		<property name="driverClass" value="${db.driverClass}"></property>
		<property name="url" value="${db.url}"></property>
		<property name="username" value="${db.username}"></property>
		<property name="password" value="${db.password}"></property>
	</bean>
	
	<!-- sqlSessionFactory -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource"></property>
		<property name="configLocation" value="classpath:mybatis-config.xml"></property>
		<property name="mapperLocations">
			<list>
				<value></value>
			</list>
		</property>
	</bean>
	
	<!-- SqlSession 설정 -->
	<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
		<constructor-arg ref="sqlSessionFactory"></constructor-arg>
	</bean>
</beans>
```

이렇게 세팅을 마쳐준 뒤 마지막으로 Mapper 를 작성하면 된다. 이 때, 조금 더 쉽게 Mapper 작성을 돕는 Plug-in Mybatipse 를 설치해주자. 이후 mybatis xml 을 생성하면 다음과 같이 Mapper 설정을 지닌 XML이 자동으로 작성된다. `mappers.article-mapper`는 XML 이름으로부터 지어진 것으로 보인다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="mappers.article-mapper"></mapper>
```

