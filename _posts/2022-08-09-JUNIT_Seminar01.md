---
title: "[Junit][세미나] TDD를 위한 Unit Test"
excerpt: "Seminar : Unit Test for TDD"
date: 2022-08-09
category:
    - junit
tag:
    - tdd
    - unit testing
    - junit
    - tdd
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
classes: wide
comments: true
---

# TDD를 위한 Unit Test 세미나

옆 모듈이 사내에서 TDD 관련 세미나를 들는다고하여 부장님께 같이 들을 수 있는지 문의드렸더니, 흔쾌히 수락해주셨다. 간단한 단위 테스트 관련된 내용을 설명해주셨고, 다음 세미나에서는 Spring Boot 예제로 심화된 내용을 전달해주신다고 했다.

## JUnit 개요

Junit 은 Java 에서 독립된 단위테스트(Unit Test)를 통해 개발자가 자주 검증을 수행할 수 있도록 지원하는 프레임워크이다. 이 Junit 을 사용하면 보이지 않고 숨겨진 테스트, 즉 통합테스트나 기능테스트 아래 함수나 메소드를 끌어내 정형화시켜 테스트를 쉽게 수행할 수 있다. 어디까지 쓰이냐면 Low-code solution 인 Mendix platform 에서도 Java 소스 코드로 표현된 Microflow 를 Junit 으로 테스트 할 수 있다고 한다.

다시 짧게 정리하자면, 단위테스트(Unit Test)란 소스 코드의 특정 모듈이 의도된 대로 정확히 작동하는지 검증하는 절차를 말하며, 모든 함수, 메소드에 대한 테스트 케이스(Test Case)를 작성하는 절차라고도 설명할 수 있다.

Junit 에 대한 기본적인 정보는 아래와 같다.

- TDD(Test Driven Development) 창시자인 Kent Beck 와 Design Pattern 책 저자인 Erich Gamma 가 작성
- 단언(Assert) 메서드로 테스트 케이스 수행 결과를 판별함
- JUnit4 부터는 테스트를 지원하는 어노테이션을 제공함
- 각 `@Test` 메서드가 호출될 때, 새로운 인스턴스를 생성해 독립적인 테스트가 이루어지도록 함

JUnit 에서 테스트를 지원하는 어노테이션(Annotation)을 간략하게 살펴보면 아래와 같다.

- `@Test`
    - `@Test`가 선언된 메서드는 테스트를 수행하는 메서드가 된다
    - JUnit은 각각의 테스트가 서로 영향을 주지 않고 독립적으로 실행됨을 원칙으로 하므로 `@Test`마다 객체를 생성함
- `@Ignore`
    - `@Ignore`가 선언된 메서드는 테스트를 실행하지 않게 함
- `@Before`
    - `@Before`가 선언된 메서드는 `@Test` 메소드가 실행되기 전에 반드시 실행됨
    - `@Test` 메소드에서 공통으로 사용하는 코드를 `@Before`에 선언해 사용할 수 있음
- `@After`
    - `@After`가 선언된 메서드는 `@Test` 메소드가 실행된 후 실행됨
- `@BeforeClass`
    - `@BeforeClass` 어노테이션은 `@Test` 보다 먼저 한 번만 수행되어야 할 경우 사용
- `@AfterClass`
    - `@AfterClass` 어노테이션은 `@Test` 메소드보다 나중에 한 번만 수행되어야 할 경우 사용

이러한 테스트 케이스의 논리를 구성하기 위한 단언(Assert) 메서드를 간단하게 아래와 같이 정리할 수 있다. `assert` 는 `~이라고 확신하다` 라는 뜻을 가진다. 따라서 단언한 조건에 부합하지 않으면 테스트를 통과하지 못 한다. 아래 다섯 가지 외에도 다양한 assert 메서드가 존재한다.

- `assertEquals(a, b)` : 객체 A와 B가 일치함을 확인함
- `assertArrayEquals(a, b)` : 배열 A와 B가 일치함을 확인함
- `assertSame(a, b)` : 객체 A와 B가 같은 객체임을 확인함. `assertEquals` 메서드는 객체의 값이 같은지 확인하는 반면 `assertSame`은 두 객체의 레퍼런스가 같은지 비교
- `assertTrue(a)` : 조건 A가 참인가를 확인
- `assertNotNull(a)` : 객체 A가 null 이 아님을 확인

Spring에서는 JUnit4를 조금 더 편리하게 사용하기 위해 Spring-Test 라이브러리를 지원한다. Spring-Test 에서 테스트를 지원하는 어노테이션(Annotation)을 이용하면 더욱 테스트가 쉬워진다.

- `@RunWith(SpringJUnit4ClassRunner.class)`
    - `@RunWith` 는 JUnit 프레임워크 테스트 실행방법을 확장할 때 사용하는 어노테이션
    - SpringJUnit4ClassRunner 라는 클래스를 지정해주면 jUnit이 테스트를 진행하는 중에 ApplicationContext 를 만들고 관리하는 작업을 자동으로 수행
    - `@RunWith` 어노테이션은 테스트 각각이 객체를 생성하더라도 싱글톤(Singleton)의 ApplicationContext 를 보장함
- `@ContextConfiguration`
    - 스프링 빈(Bean) 설정 파일 위치를 어노테이션 파라미터로 지정해줄 때 사용하는 어노테이션
- `@Autowired`
    - 스프링 DI에서 사용되는 특별한 어노테이션
    - 해당 변수에 자동으로 빈(Bean)을 매핑해줌

## jUnit4 활용 예시: Spring-test

테스트를 작성할 때는 먼저 항상 소스 코드와 동일한 디렉토리 구조를 갖추도록 한다. 그렇게 해야 구조적인 파악이 용이하기 때문이라고 한다.

```java
package com.example.hello.dao;

import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.FileSystemXmlApplicationContext;

import com.example.hello.vo.Article;

public class ArticleDAOTest {
	
	// 테스트 공통 ApplicationContext : 빈(Bean) 등록되는 곳
	private ApplicationContext ctx;
	
	@Before
	public void setup() {
		// 테스트 전 공통 작업 
		// ApplicationContext 생성
		ctx = new FileSystemXmlApplicationContext("file:src/main/webapp/WEB-INF/spring/*.xml");
	}
	
	@Test
	public void tesSelectArticleById() {
		// Test 수행 전 준비
		ArticleDAO dao = ctx.getBean("articleDAO", ArticleDAO.class);
		Article article = dao.selectArticleById(null);
		
		// Assert.assertTrue : 해당 조건이 참이라고 가정
		Assert.assertTrue(article.getAuthor().equals("lee"));
	}
}
```

위 코드는 기본적인 jUnit4 를 사용해 테스트를 수행하는 코드이다. Spring-test 를 사용해 아래와 같이 더 간단하게 작성할 수 있다.

```java
package com.example.hello.dao;

import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.FileSystemXmlApplicationContext;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import com.example.hello.vo.Article;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("file:src/main/webapp/WEB-INF/spring/*.xml")
public class ArticleDAOTest {
	
	@Autowired
	private ArticleDAO dao;
	
	@Test
	public void tesSelectArticleById() {
		Article article = dao.selectArticleById(null);
		
		// Assert.assertTrue : 해당 조건이 참이라고 가정
		Assert.assertTrue(article.getAuthor().equals("lee"));
	}
}
```

개발을 수행하며 테스트 케이스를 함께 작성하는 습관을 추천한다고 하셨는데, 실력있는 개발자가 되기 위한 길은 멀고도 험하다는 생각이 든다. 그치만 이미 사내 많은 소프트웨어들이 단위 테스트를 작성하고, 지표를 산출하고 있는 것을 보니 곧 이런 문화가 전파될 것 같다는 느낌도 어렴풋이 들었다.

## 의존성을 줄이는 방법

먼저 의존성을 줄이는데는 Mock Framework 을 사용한다. 재밌는 사실로 Junit5 는 private 메소드 호출 기능을 지원하는 Power Mock 이 호환되지 않아 Junit4 로 가이드하고 있다고 한다. 어떤 책에서는 private 메소드 호출은 최대한 자제하는 것이 좋은 테스트라고 했는데, 약간 의문이 들었다.

의존성을 줄이는 방법을 Test Double 이라고 하는데, 영화를 촬영할 때 배우를 대신해 위험한 액션을 연기하는 스턴트 더블(Stunt Double)이라는 용어에서 유래됐다고 한다. 이들은 아래와 같은 종류가 있다.

- Dummy: 아무런 동작도 하지 않으며 인스턴스화된 객체만 제공할 때 사용
- Stub: Dummy 가 실제로 동작하는 것처럼 미리 준비한 결과를 리턴할 때 사용
- Fake: 실제 동작을 단순화해 구현하는 것으로 대표적인 예로는 실제 DAO 대신 테스트 DAO 만 구현하는 경우 사용
- Spy: 가상이 아닌 실제 객체를 부분적으로 또는 완전히 사용해 동작을 검증할 때 사용
- Mock: 상태가 아닌 행위(Behavior)를 검증하는 가상 객체로 Dummy, Stub, Spy 처럼 동작 가능

