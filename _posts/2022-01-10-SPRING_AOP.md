---
title: "[Spring] 관점지향 프로그래밍 (AOP, Aspect Oriented Programming)"
excerpt: "AOP, Aspect Oriented Programming"
date: 2022-01-10
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - aop
    - aspectj
author: 1FeS
comments: true
---

# AOP

- Aspect Oriented Programming : 관점지향 프로그래밍
- 하나의 프로그램을 관점(혹은 관심사) 논리 단위로 분리해 관리하는 개념
- 로깅, 감사, 선언적 트랜젝션, 보안, 캐싱 등 다양한 곳에서 사용
- Spring Framework 에서는 관심사로 등록한 메서드가 호출되면 메서드 전과 후에 다른 메서드가 호출될 수 있도록 지원

## Spring AOP 용어

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

## XML 에서 Aspectj 사용하기

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

## 어노테이션으로 Aspectj 사용하기

앞서 XML에서 `<aop:aspect>` 태그를 이용해 Advisor 를 지정했다면, 소스 코드에서도 `@AspectJ` 어노테이션을 활용해 Advisor 역할을 할 Bean 을 설정할 수 있다. `<aop:aspectj-autoproxy/>` 태그와 `@EnableAspectJAutoProxy` 어노테이션을 사용한다. 먼저 `@Configuration`, `@ComponentScan` 어노테이션으로 설정을 담당하는 클래스 BeanConfigClass에 `@EnableAspectjAutoProxy` 코드를 추가한다. 그리고 Advisor 클래스에 `@Aspect` 어노테이션을 붙이면 끝난다.

어드바이저는 `@Before`, `@After`, `@Around`, `@AfterReturning`, `@AfterThrowing` 을 지원한다. 각 어노테이션 옆에 Execution 명시자를 활용해 관심사를 등록해 특정 메서드가 호출될 때, 함께 동작할 메서드들을 등록해 사용한다. 