---
title: "[Spring] Bean 객체 생명주기"
excerpt: "bean lifecycle"
date: 2022-01-04
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - dependency injection
    - bean
    - bean lifecycle
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Bean 객체의 생명 주기

Spring Bean 은 IOC 컨테이너가 종료될 때 객체가 소멸된다. 즉, `close()` 메소드를 이용하면 해당 컨테이너가 생성한 모든 객체를 소멸시킬 수 있다.

이처럼 객체는 생성과 소멸 과정을 거치는데, 개발자는 객체 생성과 소멸 시 호출될 메서드를 등록할 수 있다. 각 단계마다의 특징은 아래와 같다.

- 객체가 생성되면 가장 먼저 생성자가 호출된다.
- init-method : 생성자 호출 이후 자동으로 호출된다.
- destroy-method : 객체가 소멸될 때 가종으로 호출된다.
- default-init-method : init-method 를 설정하지 않은 경우 자동으로 호출된다.
- default-destroy-method : destroy-method 를 설정하지 않은 경우 자동으로 호출된다.

## init-method 사용법

- 클래스에 생성 후 사용할 메소드를 정의한다
- xml 파일 내부 bean 태그에 `init-method=" "` 에 객체의 메소드를 삽입한다

## destroy-method 사용법

- 클래스에 소멸 후 사용할 메소드를 정의한다
- xml 파일 내부 bean 태그에 `destroy-method=" "` 에 객체의 메소드를 삽입한다

## default-init-method / default-destroy-method 사용법

- 클래스에 소멸 후 사용할 메소드를 정의한다
- xml 파일 내부 상단의 beans 태그에 default 값을 삽입한다
- 같은 이름의 default-method를 가지지 않은 객체들은 무시된다
- 중복되는 경우 init-method, destroy-method 메소드가 우선순위를 가진다

## 예제 코드

**자바 코드**

```java
package com.springpractice.beans;

public class TestBean {
	public TestBean(){
		System.out.println("생성 완료");
	}
	
	public void testInit(){
		System.out.println("TestBean Init");
	}
	
	public void testDestroy() {
		System.out.println("TestBean Destroyed");
	}
}
```

**bean.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
						http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id='testBean' class='com.springpractice.beans.TestBean' init-method="testInit" destroy-method="testDestroy"></bean>	
</beans>
```

**Main 클래스**

```java
package com.springpratice.main;

import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.springpractice.beans.TestBean;

public class MainClass {

	public static void main(String[] args) {
		test3();
	}

	// ApplicationContext - 패키지 내부
	public static void test3() {
		ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("com/springpractice/config/beans.xml");
		TestBean tb1 = ctx.getBean("testBean", TestBean.class);
		System.out.printf("tb1 : %s\n", tb1);
		ctx.close();
	}
}
```

**결과**

```sh
02:31:49.473 [main] DEBUG org.springframework.context.support.ClassPathXmlApplicationContext - Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@f5f2bb7
02:31:49.847 [main] DEBUG org.springframework.beans.factory.xml.XmlBeanDefinitionReader - Loaded 1 bean definitions from class path resource [com/springpractice/config/beans.xml]
02:31:49.890 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'testBean'
생성 완료
TestBean Init
tb1 : com.springpractice.beans.TestBean@51e5fc98
02:31:49.948 [main] DEBUG org.springframework.context.support.ClassPathXmlApplicationContext - Closing org.springframework.context.support.ClassPathXmlApplicationContext@f5f2bb7, started on Tue Jan 04 02:31:49 KST 2022
TestBean Destroyed
```