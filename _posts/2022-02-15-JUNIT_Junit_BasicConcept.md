---
title: "[Junit] Junit4 활용 예제"
excerpt: "junit practical examples"
date: 2022-02-15
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

# JUnit 사용한 DI 클래스 테스트

Junit 은 Java에서 독립된 단위테스트(Unit Test)를 지원해주는 프레임 워크 또는 보이지 않고 숨겨진 단위 테스트를 끌어내 정형화시켜 단위테스트를 쉽게 해주는 테스트 지원 프레임워크로 불린다. 어디까지 쓰이냐면 Low-code solution 인 Mendix platform 에서도 Java 소스 코드로 표현된 Microflow 를 Junit 으로 테스트 할 수 있다. 

여기서 단위테스트(Unit Test)란 소스 코드의 특정 모듈이 의도된 대로 정확히 작동하는지 검증하는 절차를 말한다. 모든 함수, 메소드에 대한 테스트 케이스(Test Case)를 작성하는 절차라고도 설명할 수 있다.

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

## jUnit4 활용 예시

테스트를 작성할 때는 먼저 항상 소스 코드와 동일한 디렉토리 구조를 갖추도록 한다. 

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

개발을 수행하며 테스트 케이스를 함께 작성하는 습관을 추천한다. 실력있는 개발자가 되기 위한 길은 멀고도 험하다는 생각이 든다.