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

## 다양한 빌드 설정 관리

application.properties 를 이용하면 서로 다른 개발 설정을 만들 수 있다. 예를들어, 로컬에서 개발한 어플리케이션을 서버에서 동작시키기 위해서는 많은 변수들이 달라져야한다. 간단히만 생각해도 local DB 에서 remote DB 로의 변경과 이에 따른 URL, 계정 정보 변경이 필요하고 파일 저장소의 위치 역시 운영체제가 다른 경우 경로 정보를 수정해야 한다.

매번 vim 으로 application.properties 를 열어서 바꿔주기엔... 우린 시간이 없다. 아래는 그 번잡하고 귀찮은 작업을 조금 더 쉽게 할 수 있는 방법이다. 회사에서 몇 번의 삽질과 에러를 찾아가며 정리한 것이라 두서가 없을 수 있다는 점 참고☆

1. application-{ env }.properties 로 빌드 환경을 설정
  - data source
  - database connection
  - file server property
  - etc
2. Eclipse 기준 run > run configuration > argument 에 `-Dspring.profiles.active={ env  }` 작성
  - 기본적으로 application.properties 만 읽기 때문에 안 해주면 data source 만드는데 url 없다고 에러 남
  - 계정권한과 함께 Database 에 대한 권한도 함께 설정해줘야 에러 나지 않음
3. Project 별 build 컴파일러 설정
  - 프로젝트 속성 > Java Build Path > JRE 클릭 > Edit > Execution Environment 에서 알맞은 버전 설정