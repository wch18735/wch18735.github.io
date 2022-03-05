---
title: "[Spring] 스프링 트랜잭션 (@Transactional)"
excerpt: "spring transaction"
date: 2022-03-05
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - transaction
author: 1FeS
comments: true
---

# 트랜잭션 (Transaction)

프로그래밍 언어로 작성된 응용 프로그램은 결국 명령어의 집합으로 여길 수 있다. 이 때, 하나의 업무 단위인 트랜잭션은 `논리적으로 깨어질 수 없는 일의 흐름` 으로 이해하면 될 것 같다.

가장 흔한 예시로 입출금하는 상황을 예로 들 수 있다. 먼저 `출금`하는 작업 흐름을 간단하게 구현해보면 아래와 같다.

1. 계좌 정보 확인 (Database Connection 생성)
2. 잔액 확인 (SELECT)
3. 출금하려는 금액이 잔액 이하인지 확인
4. 유효한 출금액인 경우 출금
5. 잔액을 `현재액 - 출금액` 으로 수정 (UPDATE)

만약 위 흐름이 깨질 수 없는 단위가 아니라 각각이 분리된다면 어떨까? 바로 확인들어가보자. 계좌A 잔액이 1,000,000(원) 이 있다고 가정해보자. 

먼저 한 명이 1,000,000(원) 출금을 시켰다. 그런데 마침 해당 과정이 이상없이 수행되는 도중 4번 단계에, 누군가 1,000,000(원) 을 출금 신청했다. 아직 잔액이 수정되지 않은 상태이기 때문에 해당 요청도 수행된다. 잔액이 1,000,000(원)인 계좌에서 2,000,000(원)이 출금된 것이다.

비단 금전 관련 시스템만 위와 같은 트랜잭션 관리가 필요한 것이 아니다. 현대의 대부분의 시스템은 트랜잭션 관리를 필요로 한다.

## Spring Transaction

Spring 은 트랜잭션과 관련된 3가지 핵심 기술을 제공한다.

- Transaction synchronization (트랜잭션 동기화)
    - Thread 별로 독립된 Connection 을 사용 할 수 있도록 하는 기술
- Transaction abstraction (트랜잭션 추상화)
    - 종속적인 코드를 이용하지 않고도 일관된 트랜잭션 처리를 가능하게 하는 기술
- Declarative transaction management (선언적 트랜잭션 관리)
    - `@Transactional` 어노테이션을 이용해 트랜잭션 선언부 추출

<span style="color: gray; font-weight: bold; font-size: 1.2em">※ 작성 중 (2022-03-05 ~)</span>

## References

 - [https://goddaehee.tistory.com/167](https://goddaehee.tistory.com/167)
 - [https://mangkyu.tistory.com/154](https://mangkyu.tistory.com/154)
 - [TransactionSynchronizationManager Docs](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/support/TransactionSynchronizationManager.html)

