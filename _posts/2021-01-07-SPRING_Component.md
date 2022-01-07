---
title: "[Spring] Component"
excerpt: "component"
date: 2022-01-07
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - dependency injection
    - jsr-250 annotation
    - annotation
author: 1FeS
comments: true
---

`@Component` 어노테이션을 사용하면 Bean Configuration 파일에 Bean 을 등록하지 않아도 자동으로 등록된다. XML 파일에 Bean 등록하던 과정에서 Bean Configuration Class 소스에 등록하는 과정을 거쳐 조금 더 편리하게 빈 객체를 등록하는 방법이라고 생각하면 될 것 같다.

XML에서 사용하는 방법은 아래와 같다. component 어노테이션을 사용하는 패키지를 등록하면 해당 패키지 내부 클래스들의 어노테이션을 분석한다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
						http://www.springframework.org/schema/beans/spring-beans.xsd
						http://www.springframework.org/schema/context
						http://www.springframework.org/schema/context/spring-context.xsd">
	
	<!-- 지정된 패키지 안에 있는 Bean 클래스들의 어노테이션을 분석하도록 지정 -->
	<context:component-scan base-package="com.springpractice.components"></context:component-scan>
</beans>
```

XML과 똑같은 기능을 담당하는 파일을 소스로 만들 수 있다. Configuration 파일 위에 다음과 같이 `@ComponentScan(basePackages=" ")` 어노테이션을 등록한다. 해당 어노테이션을 여러 개 사용해 여러 개 패키지 경로를 등록할 수 있다.

```java
package com.springpractice.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

import com.springpractice.beans.DataBean1;
import com.springpractice.beans.TestBean1;
import com.springpractice.beans.TestBean2;

@Configuration
@ComponentScan(basePackages="com.springpractice.components")
public class BeanConfigClass {
	
}
```

등록된 패키지 내부에는 `@Component` 어노테이션으로 빈 객체를 생성함과 동시에 등록할 수 있다. 타입만 지정하거나 이름을 통해 등록하는 방법 모두 적용할 수 있다. 이러한 `@Component` 어노테이션은 한 클래스 당 하나만 등록할 수 있기 대문에 만약 같은 타입의 빈 객체를 서로 다른 이름으로 여러 개 등록하고 싶다면 직접 하나씩 만들어야 하는 것은 마찬가지이다.

```java
// 타입만 지정
package com.springpractice.components;

import org.springframework.stereotype.Component;

@Component
@Lazy
@Scope("prototype")
public class TestBean3 {
	
}
```

```java
package com.springpractice.components;

import org.springframework.stereotype.Component;

@Component("obj")
public class TestBean3 {
	
}
```

해당 컴포넌트에는 `@Lazy`, `@Scope`, `@postConstruct`, `@preDestroy` 등을 통해 컴포넌트 객체 속성을 지정할 수 있으며 `@Autowired`, `@Quialifier`, `@Resource`  등을 이용해 자동 주입을 구현할 수 있다.

생성자를 정의할 때는 생성자를 하나만 정의해야 에러가 발생하지 않는다. 만약 생성자를 2개 이상 정의했다면 그 중 하나에 `@Autowired` 어노테이션을 붙여야 한다. `@Autowired` 어노테이션을 둘 다 등록했다면 에러가 발생한다.