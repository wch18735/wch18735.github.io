---
title: "[Spring] Java Bean Configuration"
excerpt: "bean configuration"
date: 2022-01-06
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - dependency injection
    - bean
    - annotation
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 어노테이션을 이용한 Bean 작업

Spring에서는 xml에서 세팅했던 작업을 Java 코드 내에서 작업할 수 있도록 지원한다. Java 코드 내에서 작업하면 주입할 정보를 외부 네트워크에서 받아오거나 유연한 계산 값을 이용할 수 있는 장점이 있다. 코드는 다음과 같이 어노테이션을 이용하며 기존에 xml 에서 사용하던 태그들을 그대로 사용할 수 있다. 

## @Bean

- @Bean 어노테이션은 bean 객체를 정의할 때 사용한다
- 메서드 이름이 bean 이름이 된다
- @Bean(name=이름) : bean 이름을 새롭게 정의한다
- @Lazy : lazy-init 여부 지정
- @Scope : bean socpe 설정
- @Primary : primary 속성 지정 (동일한 클래스 타입 빈이 여러 개일 때, 한 가지를 결정)

아래에는 정리했던 어노테이션을 적용한 예시 코드다. Bean 생성할 때 사용하는 다양한 옵션들이 직관적으로 보여진다.

```java
package com.springpractice.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Lazy;
import org.springframework.context.annotation.Primary;
import org.springframework.context.annotation.Scope;

import com.springpractice.beans.TestBean1;

@Configuration
public class BeanConfigClass {
	
	@Lazy
	@Scope("prototype")
	@Bean(name="tb1")
	public TestBean1 java1() {
		TestBean1 t1 = new TestBean1();
		return t1;
	}
	
	@Primary
	@Bean
	public TestBean1 java2() {
		TestBean1 t1 = new TestBean1();
		return t1;
	}
}
```

## init, destroy 메서드

빈 생명주기에 따른 `init-method` 와 `destroy-method` 활용 역시 가능하다. 전달할 클래스 내부에 `init method` 와 `destroy method` 둘을 정의하고 @Bean 내부에 쉼표로 구분하여 아래와 같이 적어준다. 이후 MainClass를 실행해보면 빈 객체 생성과 종료시 각각 호출되는 메서드를 확인할 수 있다.

```java
package com.springpractice.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Lazy;
import org.springframework.context.annotation.Primary;
import org.springframework.context.annotation.Scope;

import com.springpractice.beans.TestBean1;

@Configuration
public class BeanConfigClass {
	
	@Lazy
	@Scope("prototype")
	@Bean(name="tb1", initMethod="init", destroyMethod="destroy")
	public TestBean1 java1() {
		TestBean1 t1 = new TestBean1();
		return t1;
	}
	
	@Primary
	@Bean(initMethod="init", destroyMethod="destroy")
	public TestBean1 java2() {
		TestBean1 t1 = new TestBean1();
		return t1;
	}
}
```