---
title: "[Spring] java.lang.ClassNotFoundException 오류"
excerpt: "java.lang.ClassNotFoundException"
date: 2022-02-19
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - aop
    - aspectj
    - jdbc
author: 1FeS
comments: true
---

`datasource.xml` 파일에 연결된 `database.properties` 파일 값을 아무리 바꿔봐도 `Could not find class [oracle.jdbc.driver.OracleDriver]` 에러가 떴다. 안 하려고 했지만.. 

`프로젝트 우클릭 > Properties > Java Build Path > Add External Jars...` 로 ojdbc8.jar 파일을 추가해줬다. 나중에 lib 파일에 묶어서 빌드해야되는 번거로움이 있지만.. 테스트가 깔끔하게 돌아가니 좋다.