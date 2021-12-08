---
title: "[Maven] org.slf4j.Logger 에러 해결 방법"
excerpt: "maven project import error org.slf4j.Logger"
date: 2021-12-01
layout: single
classes: wide
category:
    - maven
tag:
    - java
    - jsp
    - servlet
    - maven
    - maven error
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

Apache maven 은 자바용 프로젝트 관리 도구다. Apache Ant 대안으로 만들어졌으며, Apache License 로  배포되는 오픈 소스 소프트웨어다. 비슷한 도구로 Gradle 이 있으며 성능도 확실히 Gradle 이 좋다고 하지만 자료는 Maven 이 더 많은 것 같다.

Maven 을 더 자세히 말하면 빌드 도구라 할 수 있다. 참고로 빌드는 소스코드 파일을 컴퓨터에서 실행할 수 있는 독립 소프트웨어(바이너리 파일)로 바꾸는 작업이라고 한다. 개발자가 작성한 코드 (.java, .xml, .jpg, .jar, .properties) 을 JVM 이나 WAS 가 인식할 수 있는 구조로 `패키징`하는 것이 대표적인 예다.

아래는 사용 예제이다. `pom.xml`  파일에 설정 내용을 적어 놓으면 내가 사용할 라이브러리 뿐만 아니라 현재 로컬에 없는 라이브러리도 네트워크를 통해 받아준다. 이런 네트워크는 아파치 재단에서 운영하는 중앙 저장소 또는 특정 회사만의 저장소 모두를 포함한다.

```xml
<project>
  <!-- model version is always 4.0.0 for Maven 2.x POMs -->
  <modelVersion>4.0.0</modelVersion>

  <!-- project coordinates, i.e. a group of values which
       uniquely identify this project -->

  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0</version>

  <!-- library dependencies -->

  <dependencies>
    <dependency>

      <!-- coordinates of the required library -->

      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>

      <!-- this dependency is only used for running and compiling tests -->

      <scope>test</scope>

    </dependency>
  </dependencies>
</project>
```

더 자세한 내용은 [여기](https://goddaehee.tistory.com/199)에 있는데 나중에 더 자세히 알아보고 정리할 예정.

위 방법을 이용해 `org.slf4j.Logger` 관련 에러를 해결할 수 있다. `pom.xml` 에 아래처럼 설정을 추가해보자. 곧 Eclipse 가 자동으로 라이브러리를 업데이트한다.

```xml
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.25</version>
</dependency>
```

참고로 Eclipse 에서 maven dependency injection (라이브러리 추가) 하는 방법은 아래 2가지가 있다. 사실 더 많을 수도 있는데 나는 일단 2개만 안다.

1. Eclipse project explorer > Project right click > maven > Add Dependency
2. pom.xml > dependecies tag > dependency tag > add library
```xml
<dependencies>
  	<dependency>
  		<groupId>org.apache.commons</groupId>
  		<artifactId>commons-compress</artifactId>
  		<version>1.16.1</version>
  	</dependency>
</dependencies>
```