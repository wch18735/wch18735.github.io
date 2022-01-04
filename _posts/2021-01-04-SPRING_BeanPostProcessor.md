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

Bean 객체를 정의할 때, init-method 속성을 설정하면 객체가 생성될 때 자동으로 호출될 메소드를 지정할 수 있다. 이 때, BeanPostProcessor 인터페이스를 구현한 클래스를 정의하면 Bean 객체를 생성할 때 호출될 init 메서드 호출을 가로채 다른 메서드를 호출할 수 있다.

- postProcessBeforeInitialization : init-method 에 지정된 메서드가 호출되기 전에 호출
- postProcessAfterInitialization : init-method 에 지정된 메서드가 호출된 후에 호출
- init-method 가 지정되지 않더라도 자동 호출

이를 사용하는 방법은 다음과 같다.

1. BeanPostProcessor 인터페이스를 implements 하는 객체 선언
2. postProcessBeforeInitialization 오버라이드
3. postProcessAfterInitialization 오버라이드
4. xml 에 bean 태그 이용한 객체 생성

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
						http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id='t1' class='com.springpractice.beans.TestBean1' init-method="bean1_init" lazy-init="true"></bean>
	<bean id='t2' class='com.springpractice.beans.TestBean2' init-method="bean2_init" lazy-init="true"></bean>	
	<bean class='com.springpractice.processor.TestBeanPostProcessor'></bean>
</beans>
```

```java
// BeanPostProcessor.java
package com.springpractice.processor;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;

public class TestBeanPostProcessor implements BeanPostProcessor{
	
	// init-method 호출 전
	@Override
	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		switch(beanName) {
		case "t1":
			System.out.println("id 가 t1 인 bean 객체 생성");
			break;
		case "t2":
			System.out.println("id 가 t2 인 bean 객체 생성");
			break;
		}
		return bean;
	}
	
	// init-method 호출 후
	@Override
	public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		System.out.println("after");
		return bean;
	}
}
```

```java
// Main.java
package com.springpratice.main;

import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.springpractice.beans.TestBean1;
import com.springpractice.beans.TestBean2;

public class MainClass {

	public static void main(String[] args) {
		test();
	}

	public static void test() {
		ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("com/springpractice/config/beans.xml");
		TestBean1 tb1 = ctx.getBean("t1", TestBean1.class);
		System.out.printf("tb1 : %s\n", tb1);
		
		TestBean2 tb2 = ctx.getBean("t2", TestBean2.class);
		System.out.printf("tb2 : %s\n", tb2);
		
		ctx.close();
	}
}
```

```sh
# Result
14:31:01.954 [main] DEBUG org.springframework.context.support.ClassPathXmlApplicationContext - Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@f5f2bb7
14:31:02.205 [main] DEBUG org.springframework.beans.factory.xml.XmlBeanDefinitionReader - Loaded 3 bean definitions from class path resource [com/springpractice/config/beans.xml]
14:31:02.250 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'com.springpractice.processor.TestBeanPostProcessor#0'
14:31:02.302 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 't1'
TestBean1 생성자
id 가 t1 인 bean 객체 생성
TestBean1 init 메서드
after
tb1 : com.springpractice.beans.TestBean1@5276e6b0
14:31:02.307 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 't2'
TestBean2 생성자
id 가 t2 인 bean 객체 생성
TestBean2 init 메서드
after
tb2 : com.springpractice.beans.TestBean2@71b1176b
14:31:02.324 [main] DEBUG org.springframework.context.support.ClassPathXmlApplicationContext - Closing org.springframework.context.support.ClassPathXmlApplicationContext@f5f2bb7, started on Tue Jan 04 14:31:01 KST 2022
```