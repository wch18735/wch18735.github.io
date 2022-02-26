---
title: "[Spring Boot] Simple API"
excerpt: "really really simple API"
date: 2022-02-27
layout: single
classes: wide
category:
    - spring boot
tag:
    - spring
    - interceptors
    - transaction
author: 1FeS
comments: true
---

Spring Boot... 넘사다 싶을 정도다. 그냥 Spring Initializer 로 dependency 몇 개 추가해서 받아오니까 API 하나 뚝딱이네.. 무엇을 위해 maven central repository 를 뒤지고 다녔지.

```java
// PangtudyBackendBootApplication.java
package com.pangtudy.pangtudybackendboot;

@SpringBootApplication
@RestController
public class PangtudyBackendBootApplication {

	public static void main(String[] args) {
		SpringApplication.run(PangtudyBackendBootApplication.class, args);
	}
	
	@GetMapping("/")
	public String hello() {
		return "Hello World";
	}
}
```

이러면 그냥 끝. 127.0.0.1:8080 으로 쏘면 그냥 나온다. 허허... 