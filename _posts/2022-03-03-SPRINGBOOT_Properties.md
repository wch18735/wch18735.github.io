---
title: "[Spring Boot] .properties 파일"
excerpt: ".properties 파일"
date: 2022-03-03
layout: single
classes: wide
category:
    - spring boot
tag:
    - spring
    - interceptors
    - properties
author: 1FeS
comments: true
---

# 프로퍼티스 파일 (.properties)

`.properties` 는 응용 프로그램의 구성 가능한 파라미터들을 저장하기 위해 사용되는 파일 확장자이다. Globalization, Localization 을 위한 문자열을 모두 저장하는데 사용할 수 있으며 이러한 성질을 Property Resource Bundles 라 부른다.

최근엔 `.yml` 로 설정 값을 저장하는 방법을 더 많이 선호한다. prefix 를 중복 기재하지 않아도 되며, 계층형으로 선언되어 사람이 읽기 편한 형태로 관리할 수 있다는 것이 장점이다.

반면 엄격한 작성 기준이 요구되어 one missing space 에도 동작하지 않을 수 있다는 단점이 있다고 한다.

## @Value 활용 예시

먼저 `.properties` 파일에 아래와 같이 내용을 기재한다. 이렇게 기재한 값은 key-value 쌍으로 저장된다.

```properties
# Test properties
say-hello=Say Hello to my friends!
```

이 값을 불러와서 사용하는 방법이 바로 `@Value` 이다. 아래와 같이 `@Value` 으로 Class 내부 변수에 값을 전달할 수 있다.

```java
package com.pangtudy.pangtudybackendboot;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class PangtudyBackendBootApplication {
	
	@Value("${say-hello}")
	String sayHello;
	
	public static void main(String[] args) {
		SpringApplication.run(PangtudyBackendBootApplication.class, args);
	}

	@GetMapping("/")
	public String hello() {
		System.out.println(sayHello);
		return "Hello World";
	}
}
```

물론 `.xml` 타입으로 관리되는 설정 파일에도 동일하게 활용할 수 있다. 아래는 DataSource 정보가 기재된 `database.properties` 파일에 미리 기재된 값을 전달하는 방법이다. 

```xml
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
```

Spring Boot 에서 공통으로 관리되는 값들은 [공식 사이트](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties)에서 확인 할 수 있다.