---
title: "[Pangtudy Web] File Upload Issue (3)"
excerpt: "pnagtudy 개발 일지"
date: 2022-03-03
category:
    - pangtudy web
tag:
    - web development
    - spring boot
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# Trouble Shooting

<span style="font-size: 1.2em; font-weight: bold">Adding JUnit4</span>

`.properties` 파일에 값을 세팅해 Database 에 연결되는지 확인해보려고 테스트 코드를 작성하려고 했다. 근데 Spring Boot 버전이 2.6 snapshot 인데, JUnit5 가 Default 로 잡혀있고, JUnit4에서 사용하는 Annotation 은 사용 못 하는 문제가 있네. 

검색해봤더니 아래 dependency 를 pom.xml 에 추가하는 방법으로 해결할 수 있다고 했다. 이전에는 exclude 로 빼내서 사용해야 되는데, 지금은 기본적으로 빼고 사용하는 것 같음.

JUnit4 가 vintage, JUnit5 는 jupiter 인 듯.

```xml
<dependency>
    <groupId>org.junit.vintage</groupId>
    <artifactId>junit-vintage-engine</artifactId>
</dependency>
```

<span style="font-size: 1.2em; font-weight: bold">JPA? MyBatis?</span>

MyBatis 쓰면 앞으로 나랑 얘기 안 한다는 동료 개발자의 말에 쿨하게 JPA 쓰기로 결정.