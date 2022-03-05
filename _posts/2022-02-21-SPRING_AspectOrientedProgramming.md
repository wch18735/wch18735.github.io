---
title: "[Spring] Aspect Oriented Programming"
excerpt: "aspect oriented programming"
date: 2022-03-05
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

# AOP 개요

어플리케이션 상에서 기능은 핵심 기능과 부가 기능으로 나눌 수 있다. 업무 로직을 포함하는 기능을 핵심 기능(Core Concerns) 라고 하며 핵심기능을 도와주는 부가적인 기능(로깅, 보안 등)을 부가 기능(Cross-cutting Concerns)이라고 부른다.

AOP는 어플리케이션에서의 관심사의 분리(기능 분리)를 뜻한다. 즉, 핵심적인 기능에서 부가 기능을 분리해 따로 관리할 수 있도록 하는 것이다. 이렇게 분리한 부가 기능을 애스펙트 (Aspect)라는 모듈 형태로 만들어 설계하고 관리하는 것을 AOP라 한다.

OOP를 적용해도 핵심 기능에서 부가 기능을 쉽게 분리된 모듈로 작성하기 어려운 문제점을 AOP가 해결해 준다고 볼 수 있다. AOP는 부가 기능을 애스펙트(Aspect)로 정의해 핵심 기능에서 부가 기능을 분리할 수 있도록 도와 객체지향적 가치를 지킬 수 있도록 돕는다.

이를 정리해보면 아래와 같이 나타낼 수 있다.

- Aspect Oriented Programming : 관점지향 프로그래밍
- 하나의 프로그램을 관점(혹은 관심사) 논리 단위로 분리해 관리하는 개념
- 로깅, 감사, 선언적 트랜젝션, 보안, 캐싱 등 다양한 곳에서 사용
- Spring Framework 에서는 관심사로 등록한 메서드가 호출되면 메서드 전과 후에 다른 메서드가 호출될 수 있도록 지원

결론적으로 관점은 부가 기능을 정의한 코드인 어드바이스(Advice)와 어드바이스를 어디에 적용할지 결정하는 포인트컷(Point Cut)을 합친 개념이다.

> Advice + Point Cut = Aspect

AOP 개념을 적용하면 핵심 기능 코드 사이에 침투된 부가 기능을 독립적인 관점들로 구분해 낼 수 있다. 이렇게 구분된 부가 기능 관점을 런타임 시에 필요한 위치에 동적으로 참여하게 만들 수 있다.

## AOP 용어

- 타깃 (Target) : 핵심 기능을 담고있는 모듈로, 타깃은 부가 기능을 부여할 대상이 됨
- 어드바이스 (Advice) : 어드바이스는 타깃에 제공할 부가기능을 담고있는 모듈
- 조인 포인트 (Join Point) : 어드바이스가 적용될 수 있는 위치를 뜻한다. 타깃 객체가 구현한 인터페이스의 모든 메서드는 조인 포인트가 된다.
- 포인트컷 (Point Cut) : 어드바이스를 적용할 타깃의 메서드를 선별하는 정규 표현식이다. 포인트컷 표현식은 execution 으로 시작하고, 메서드의 Signature 를 비교하는 방법을 주로 이용한다.
- 어드바이저 (Advisor) : 어드바이저는 Spring AOP 에서만 사용되는 특별한 용어로 `어드바이스 + 포인트 컷` 을 뜻한다.
- 위빙 (Weaving) : 위빙은 포인트컷에 의해서 결정된 타깃의 조인 포인트에 부가 기능을 삽입하는 과정을 뜻한다. 위빙은 AOP가 핵심 기능(타깃)의 코드에 영향을 주지 않으면서 필요한 부가기능(어드바이스)을 추가할 수 있도록 해주는 핵심적 처리 과정이다.

<span style="font-size: 1.2em; color: gray">Spring AOP 용어</span>

- Join Point : 모듈이 삽입되어 동작할 수 있는 특정 위치 (관심사, 관심 대상)
- Point Cut : Joint Point 에 어떤 것을 삽입할지 선택하는 행위 (많은 Join Point 중 관심사에 맞게 선택하는 행위)
- Advice : Joint Point 에 삽입해 동작하는 코드 (Join Point 앞 뒤로 삽입 될 모듈)
  - before : 메서드 호출 전에 동작하는 Advice
  - after-returning : 예외 없이 호출된 메서드의 동작이 완료되면 동작하는 Advice
  - after-throwing : 호출된 메서드 동작 중 예외가 발생했을 때 동작하는 Advice
  - after : 예외 발생 여부에 관계없이 호출된 메서드의 동작이 완료되면 동작하는 Advice
  - around : 메서드 호출 전과 후에 동작하는 Advice
- Weaving : Advice 를 핵심 로직 코드에 적용하는 것 (Advice 를 Join Point 앞 뒤로 연결하는 행위)
- Aspect : Point Cut + Advice

위 용어들을 그림으로 정리하면 아래와 같이 표현할 수 있다.

<img src="/_img/2022-02-21/aop_concept.png">

Spring Framework 에서 관점지향 프로그래밍을 수행하려면 먼저 aspectj dependency 추가를 해주어야 한다. IoC 와 DI 를 수행했던 것과 마찬가지로 AOP 적용 역시 `1) XML 에서 하는 방식`과 `2) 어노테이션을 이용하는 방법` 두 가지가 존재한다.

```xml
<!-- version : 1.9.4 -->
<!-- aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>${aspectjweaver-version}</version>
</dependency>
```

## Spring AOP 특징

Spring은 프록시(Proxy) 기반 AOP를 지원한다. 타깃 객체에 대한 프록시를 만들어 제공하는 형태이다. 타깃을 감싸는 프록시는 Runtime 단계에 생성되며, 어드바이스를 타깃 객체에 적용하면서 생성되는 객체다.

이렇게 생성된 프록시(Proxy)는 호출을 가로챈다. 이를 Intercept 라고 한다. 프록시는 타깃 객체에 대한 호출을 가로챈 다음 어드바이스의 부가기능 로직을 수행하고 난 후 타깃의 핵심기능 로직을 호출한다. 반대로 타깃의 핵심기능 로직 메서드를 호출한 후 부가기능을 수행하는 경우도 있다. `@Before` 와  `@After` 로 지정할 수 있다.

<img src="/_img/2022-02-21/aop_proxy.png">

Spring은 동적 프록시를 기반으로 AOP를 구현한다. 따라서 Spring AOP는 메서드 조인 포인트만 지원한다. 즉, 핵심기능(타깃)의 메서드가 호출되는 런타임 시점에만 부가기능(어드바이스)을 적용할 수 있다. 반면 AspectJ 같은 고급 AOP 프레임워크를 사용하면 객체 생성, 필드값 조회 및 조작, Static 메서드 호출 및 초기화 등 다양한 작업에 부가 기능을 적용할 수 있다.

## 활용 예시

아래는 관점 지향 프로그래밍의 예시들을 간단하게 정리한 것이다.

### Aspectj 사용하기 : XML

먼저 xml configuration 파일에서 AOP을 수행하는 방법이다. 아래와 같이 설정 파일 상단에 네임스페이스를 추가해준다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
						http://www.springframework.org/schema/beans/spring-beans.xsd
						http://www.springframework.org/schema/context
						http://www.springframework.org/schema/context/spring-context.xsd
						http://www.springframework.org/schema/aop
						http://www.springframework.org/schema/aop/spring-aop.xsd">
			
</beans>
```

예제를 위한 기본 환경 설정으로 아래와 같은 TestBean 클래스를 작성한다. 클래스 내부 메소드를 하나 정의하고 `Joint Point` 로 설정할 것이다. 해당 `Joint Point`에 `Advice`를 삽입하는 다양한 예시를 확인함으로써 관점 지향 프로그래밍이 무엇을 추구하는지 감을 익힐 수 있다.

```java
package com.springpractice.beans;

public class TestBean {
	public int method1(){
		System.out.println("메소드 호출");
		return 100;
	}
}
```

이렇게 작성한 클래스를 IoC에 빈으로 등록하고 MainClass 에서 불러와 `method1()`을 호출한다. TestBean 클래스 `method1()` 메서드에 작성된 `return 100;` 반환문에 의해 `x1` 에 100이 할당된다.

```java
package com.springpractice.main;

import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.springpractice.beans.TestBean;
import com.springpractice.config.ConfigClass;

public class MainClass {

	public static void main(String[] args) {
		ClassPathXmlApplicationContext cpctx = new ClassPathXmlApplicationContext("com/springpractice/config/beans.xml");
		TestBean xml1 = cpctx.getBean("xml1", TestBean.class);
		int x1 = xml1.method1();
		System.out.println(x1);
		cpctx.close();
    }
}
```

XML에 메서드 호출을 관심사로 지정하고, 관심사가 호출될 때마다 특정한 메서드가 전 또는 후에 함께 호출되도록 코드를 작성한다. 앞에서 `pointcut`을 통해 설정한 관심사가 발생했을 때, `Advice`를 삽입하는 클래스 `Advisor` 를 먼저 작성한다. 메소드 호출 전, 후, 앞 뒤, 정상 동작 후, 비정상 동작 후 등으로 호출되는 경우를 나눌 수 있다.

```java
package com.springpractice.advisor;

import org.aspectj.lang.ProceedingJoinPoint;

public class AdvisorClass {
	public void beforeMethod() {
		System.out.println("beforeMethod 메소드 호출");
	}
	
	public void afterMethod() {
		System.out.println("afterMethod 메소드 호출");
	}
	
	public Object aroundMethod(ProceedingJoinPoint pjp) throws Throwable {
		System.out.println("around 메소드 호출1");
		
        // Original Method Call
		Object obj = pjp.proceed();
		
		System.out.println("aroundMethod 호출2");
		
		return obj;
	}
	
	public void afterReturningMethod() {
		System.out.println("afterReturningMethod 호출");
	}
	
	public void afterThrowingMethod(Throwable e) {
		System.out.println("afterThrowingMethod 호출");
		System.out.println(e);
	}
}
```

이렇게 작성한 클래스를 XML Configuration 파일에 다음과 같이 작성해 활용한다. 각 태그를 해석해보면 다음과 같다.

- `<aop:aspect ref='advisor1'>` : IoC에 `advisor1` 으로 등록된 빈을 advisor 로 등록한다. 
- `<aop:pointcut expression="execution(* method1())" id="point1"/>` : `* method1()` 이 발생하는 point 들에 `id=point1`을 부여한다
- `<aop:before method="beforeMethod" pointcut-ref="point1"/>` : `point1` 이라는 아이디를 가진 pointcut 이 발생하기 `before` 에 `advisor1` 내부의 `beforeMethod` 로 선언된 메소드를 호출한다.
- ...

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
						http://www.springframework.org/schema/beans/spring-beans.xsd
						http://www.springframework.org/schema/context
						http://www.springframework.org/schema/context/spring-context.xsd
						http://www.springframework.org/schema/aop
						http://www.springframework.org/schema/aop/spring-aop.xsd">
						
	<bean id='xml1' class='com.springpractice.beans.TestBean'></bean>
	
	<bean id='advisor1' class='com.springpractice.advisor.AdvisorClass'></bean>
	
	<aop:config>
		<aop:aspect ref='advisor1'>
			<aop:pointcut expression="execution(* method1())" id="point1"/>
			
			<aop:before method="beforeMethod" pointcut-ref="point1"/>
			<aop:after method="afterMethod" pointcut-ref="point1"/>
			<aop:around method="aroundMethod" pointcut-ref="point1"/>
			<aop:after-returning method="afterReturningMethod" pointcut-ref="point1"/>
			<aop:after-throwing method="afterThrowingMethod" pointcut-ref="point1" throwing="e"/>
		</aop:aspect>
	</aop:config>
</beans>
```

대다수 사람들이 문서 대부분에 익숙할 것이다. 그러나 분명 한 가지 생소한 부분을 느낄 것 같다. `expression="execution(* method1())"` 이다. 이 부분을 Execution 명시자라고 하는데 다음과 같이 간략하게 정리할 수 있다.

- Pointcut 을 지정할 때 사용하는 문법
- execution( [접근 제한자] [리턴 타입] [클래스 이름] [메서드 이름] (매개 변수) )
- 접근 제한자 : public 제한자만 지원
- 리턴 타입 : 메서드 매개변수 타입
- 클래스 이름 : 패키지를 포함한 클래스 이름
- 메서드 이름 : 클래스 내부 메서드 이름
- 매개변수 : 매개변수 형태
- `*` : 모든 것
- `..` : 개수 상관없이 모든 것

따라서 아래와 같은 다양한 형태로 활용할 수 있다.

- `<aop:pointcut id="point1" expression="execution(public void com.springpractice.beans.TestBean.method())"/>`
- `<aop:pointcut id="point1" expression="execution(void com.springpractice.beans.TestBean.method(java.lang.String))"/>`
- `<aop:pointcut id="point1" expression="execution(void com.springpractice.beans.TestBean.method())"/>`
- `<aop:pointcut id="point1" expression="execution(void com.springpractice.beans.TestBean.method(*))"/>`
- `<aop:pointcut id="point1" expression="execution(int com.springpractice.beans.TestBean.method(int, int))"/>`
- `<aop:pointcut id="point1" expression="execution(void com.springpractice.beans.TestBean.method(..))"/>`
- `<aop:pointcut id="point1" expression="execution(void com.springpractice.beans.TestBean.*())"/>`
- `<aop:pointcut id="point1" expression="execution(void com.springpractice.beans.*.method())"/>`
- `<aop:pointcut id="point1" expression="execution(*(..))"/>`

### Aspectj 사용하기 : Annotation

앞서 XML에서 `<aop:aspect>` 태그를 이용해 Advisor 를 지정했다면, 소스 코드에서도 `@AspectJ` 어노테이션을 활용해 Advisor 역할을 할 Bean 을 설정할 수 있다. `<aop:aspectj-autoproxy/>` 태그와 `@EnableAspectJAutoProxy` 어노테이션을 사용한다. 먼저 `@Configuration`, `@ComponentScan` 어노테이션으로 설정을 담당하는 클래스 BeanConfigClass에 `@EnableAspectjAutoProxy` 코드를 추가한다. 그리고 Advisor 클래스에 `@Aspect` 어노테이션을 붙이면 끝난다.

어드바이저는 `@Before`, `@After`, `@Around`, `@AfterReturning`, `@AfterThrowing` 을 지원한다. 각 어노테이션 옆에 Execution 명시자를 활용해 관심사를 등록해 특정 메서드가 호출될 때, 함께 동작할 메서드들을 등록해 사용한다.

트랜잭션(Transaction, 이하 트랜잭션)이란 Interrupt 없이 수행되는 일련의 작업 단위이다. 특히 데이터베이스 필드에서는 데이터베이스 상태를 변화시키는 작업 단위를 뜻한다.

예를 들어, 내가 게시글을 포스팅하는 순간을 나열해보자. 

- INSERT 문이 수행되고
- 화면이 게시글 목록으로 돌아온 뒤
- 현재 게시글을 SELECT 해와
- 화면에 보여준다

각각의 단계는 나뉘어져 있으나 `논리적으로는 모두 하나의 흐름으로서 진행`되어야 한다. 이것을 트랜잭션이라 하며 원자성(Atomicity), 일관성(Consistency), 독립성(Isolation), 지속성(Durability) 네 가지 성질을 가진다.

### 트랜잭션 (Transaction)

스프링에서 역시 논리적인 하나의 흐름을 제어할 수 있는 트랜잭션 관리 기능을 제공한다. 이는 어노테이션 선언적 방식과 AOP 설정을 이용한 선언적 방식을 통해 수행된다. 프로그램에 트랜잭션 처리를 맡기는 것은 권장되지 않는다.

어노테이션을 이용한 트랜잭션 선언은 `@Transactional` 을 관리가 필요한 클래스나 메소드 상단에 추가해줌으로써 가능하다. AOP를 이용한 트랜잭션 관리는 Spring 에서 제공하는 DSTM(DataSourceTransactionManager) 클래스 타입의 어드바이저를 선언해 사용할 수 있다.

### 로깅 (SLF4J and LogBack)

Java 진영에는 commons-logging, log4j, java util logging, logback 등 많은 로깅툴이 존재한다. 이전에는 log4j와 commnos-logging 이 주로 사용되었고, Spring은 commons-logging을 내부적으로 사용해 로그를 남기고 있다.

그러나 SLF4J가 나온 후 많은 오픈소스 프로젝트에서 SLF4J 구현체인 LogBack을 이용한다고 한다. 실제로 2021년 말이었나, 2022년 초 log4j 보안 이슈가 터져서 난리가 난 경험이 다들 있을 것 같다.

Logging Tool이 혼재되면 프로젝트 빌드나 런타임 시에 충돌하는 경우가 빈번하기 때문에 이를 통일시켜주는 작업이 필요하다. 기호에 맞는 Logging Tool을 사용하되 하나로 통일해주는 것이 좋겠다.