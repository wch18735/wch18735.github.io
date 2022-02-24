---
title: "[Spring] Transaction and Logging"
excerpt: "transaction and logging"
date: 2022-02-25
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - aop
    - aspectj
    - transaction
author: 1FeS
comments: true
---

# Transaction

트랜잭션(Transaction, 이하 트랜잭션)이란 Interrupt 없이 수행되는 일련의 작업 단위이다. 특히 데이터베이스 필드에서는 데이터베이스 상태를 변화시키는 작업 단위를 뜻한다.

예를 들어, 내가 게시글을 포스팅하는 순간을 나열해보자. 

- INSERT 문이 수행되고
- 화면이 게시글 목록으로 돌아온 뒤
- 현재 게시글을 SELECT 해와
- 화면에 보여준다

각각의 단계는 나뉘어져 있으나 `논리적으로는 모두 하나의 흐름으로서 진행`되어야 한다. 이것을 트랜잭션이라 하며 원자성(Atomicity), 일관성(Consistency), 독립성(Isolation), 지속성(Durability) 네 가지 성질을 가진다.

## Spring Transaction

스프링에서 역시 논리적인 하나의 흐름을 제어할 수 있는 트랜잭션 관리 기능을 제공한다. 이는 어노테이션 선언적 방식과 AOP 설정을 이용한 선언적 방식을 통해 수행된다. 프로그램에 트랜잭션 처리를 맡기는 것은 권장되지 않는다.

어노테이션을 이용한 트랜잭션 선언은 `@Transactional` 을 관리가 필요한 클래스나 메소드 상단에 추가해줌으로써 가능하다. AOP를 이용한 트랜잭션 관리는 Spring 에서 제공하는 DSTM(DataSourceTransactionManager) 클래스 타입의 어드바이저를 선언해 사용할 수 있다.

## SLF4J and LogBack

Java 진영에는 commons-logging, log4j, java util logging, logback 등 많은 로깅툴이 존재한다. 이전에는 log4j와 commnos-logging 이 주로 사용되었고, Spring은 commons-logging을 내부적으로 사용해 로그를 남기고 있다.

그러나 SLF4J가 나온 후 많은 오픈소스 프로젝트에서 SLF4J 구현체인 LogBack을 이용한다고 한다. 실제로 2021년 말이었나, 2022년 초 log4j 보안 이슈가 터져서 난리가 난 경험이 다들 있을 것 같다.

Logging Tool이 혼재되면 프로젝트 빌드나 런타임 시에 충돌하는 경우가 빈번하기 때문에 이를 통일시켜주는 작업이 필요하다. 기호에 맞는 Logging Tool을 사용하되 하나로 통일해주는 것이 좋겠다.

