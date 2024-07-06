---
title: "[Spring] BeanPostProcessor"
excerpt: "BeanPostProcessor"
date: 2022-01-04
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - dependency injection
    - bean
    - bean post processor
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# BeanPostProcessor

## init-method

XML로 Bean을 정의하는 방식은 아주 오래된 코드에서나 볼 수 있을 것이다. 가령, 운이 좋아 과거 코드를 전환하는 프로젝트를 맡는다면 어노테이션이 아닌 XML 정의 방식을 만날 수 있다.

Bean 객체를 XML 에서 정의할 때, `@PostConstruct` 역할을 하는 메소드를 `init-method` 속성을 통해 설정할 수 있다. 이렇게 작성하면 객체가 생성될 때 함께 동작하는 메소드를 정의해 놓을 수 있다. Map을 초기화 해 놓을 수도 있고, 서버와 연결을 체크하는 등 다양한 기능을 삽입해 놓을 수 있다.

```xml
<bean id='t1' class='com.springpractice.beans.TestBean1' init-method="init" lazy-init="true"></bean>
```

## BeanPostProcessor

이 때, `BeanPostProcessor` 인터페이스를 구현한 클래스를 정의하면 Bean 객체를 생성하는 과정에서 호출하는 init 메서드 호출을 **한 번 더** 가로채 다른 메서드를 호출할 수 있다. 알아둘 점은 이 설정은 **모든 Bean 객체**를 대상으로 적용된다는 점이다.

- `postProcessBeforeInitialization`: init-method 에 지정된 메서드가 호출되기 전에 호출
- `postProcessAfterInitialization` : init-method 에 지정된 메서드가 호출된 후에 호출

이 두 설정은 `init-method` 속성에 아무것도 지정되어있지 않아도 동작한다. 이를 사용하는 방법을 조금 더 자세히 확인해보자.

### implements BeanPostProcessor

먼저 BeanPostProcessor 인터페이스를 구현하도록 선언한다. 이 인터페이스에는 `postProcessBeforeInitialization` 메소드와 `postProcessAfterInitialization` 메소드가 선언되어 있다.

```java
public class TestBeanPostProcessor implements BeanPostProcessor{
	// ...
}
```

### override methods

선언된 `postProcessBeforeInitialization` 메소드와 `postProcessAfterInitialization` 메소드를 구현해 오버라이드 한다.

```java
public class TestBeanPostProcessor implements BeanPostProcessor{
	@Override
	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		switch(beanName) {
		case "t1":
			System.out.println("create bean with id t1");
			break;
		case "t2":
			System.out.println("create bean with id t2");
			break;
		}
		return bean;
	}
	
	@Override
	public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		System.out.println("after init-method");
		return bean;
	}
}
```

### XML setting

Bean을 관리하는 XML에 정의한 반을 등록해준다. `TestBeanPostProcessor` 가 등록된 것을 확인할 수 있으며, 앞서 말한 것처럼 이 설정은 모든 Bean의 생명 주기에 관여한다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
						http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id='t1' class='com.springpractice.beans.TestBean1' init-method="init" lazy-init="true"></bean>
	<bean id='t2' class='com.springpractice.beans.TestBean2' init-method="init" lazy-init="true"></bean>	
	<bean class='com.springpractice.processor.TestBeanPostProcessor'></bean>
</beans>
```

## Main 동작

실제 main 코드를 살펴보자. 전환하고자 하는 대상 레거시 코드는 아래 예시로 작성한 메인보다 훨씬 더 복잡하게 구성되어 있을 것이다. 

소스를 분석하는 과정에서 메인에 선언되지 않은 동작들이 로그에서 튀어나오는게 보인다면, 앞서 설명한 설정들이 프로젝트 어딘가에 삽입되어 있다고 생각하면 되겠다.

```java

public class MainClass {

	public static void main(String[] args) {
		test();
	}

	public static void test() {
		ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("com/springpractice/config/beans.xml");

		TestBean1 tb1 = ctx.getBean("t1", TestBean1.class);
		TestBean2 tb2 = ctx.getBean("t2", TestBean2.class);
		
		ctx.close();
	}
}
```

코드에 작성된 내용을 로그에서 살펴보자. 앞서 `init-method` 속성에 설정한 메소드가 동작하기 전 `create bean with id ...` 을 통해 생성하는 Bean 별로 서로 다른 동작을 취해줄 수 있음을 알 수 있다. 

또한 `init` 동작이 끝난 후 `after init-method` 가 로그에 남는 것을 통해 설정된 초기화 작업이 끝난 후 동작하는 메소드 역시 등록된 것도 확인할 수 있다.

```sh
# ...
shared instance of singleton bean 't1'
TestBean1 생성자
create bean with id t1
TestBean1 init 메서드
after init-method
# ...
shared instance of singleton bean 't2'
TestBean2 생성자
create bean with id t2
TestBean2 init 메서드
after init-method
# ...
```

## 어디에 쓸 수 있을까?

Spring Boot 을 사용할 때, 이를 직접 건드리며 설정할 일은 크게 없을 것 같다. 그럼에도 만약 소프트웨어 신뢰성을 위해 반드시 특정 조건하에만 생성할 수 있도록 정책을 강제해야 하는 경우가 있다면 이같은 설정 Bean 도입을 고려해 볼 필요가 있겠다.