---
title: "[Spring] pom.xml"
excerpt: "pom.xml"
date: 2022-01-17
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - pom.xml
author: 1FeS
comments: true
---

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.springpractice</groupId>
	<artifactId>JDBC</artifactId>
	<version>0.0.1-SNAPSHOT</version>

	<!-- xml 에서 사용할 속성들 -->
	<properties>
		<java-version>1.8</java-version>
		<org.springframework-version>5.1.9.RELEASE</org.springframework-version>
		<org.slf4j-version>1.7.32</org.slf4j-version>
		<ch.qos.logback-version>1.2.10</ch.qos.logback-version>
		<javax.annotation-version>1.3.2</javax.annotation-version>
		<aspectjweaver-version>1.9.6</aspectjweaver-version>
		<com.oracle-version>12.2.0.1</com.oracle-version>
		<org.apache.commons-version>2.7.0</org.apache.commons-version>
		<org.mybatis-version>3.5.2</org.mybatis-version>
		<org.mybatis-spring-version>2.0.2</org.mybatis-spring-version>
	</properties>

	<!-- Repository 정보 -->
	<repositories>
		<repository>
			<id>oracle</id>
			<name>ORACLE JDBC Repository</name>
			<url>http://code.lds.org/nexus/content/groups/main-repo</url>
		</repository>
	</repositories>

	<!-- 프로젝트에서 사용할 라이브러리 정보 -->
	<dependencies>
		<dependency>
			<!-- spring-context -->
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>

		<!-- slf4j-api -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${org.slf4j-version}</version>
		</dependency>

		<!-- logback-classic -->
		<dependency>
			<groupId>ch.qos.logback</groupId>
			<artifactId>logback-classic</artifactId>
			<version>${ch.qos.logback-version}</version>
			<exclusions>
				<exclusion>
					<groupId>org.slf4j</groupId>
					<artifactId>slf4j-api</artifactId>
				</exclusion>
			</exclusions>
			<scope>runtime</scope>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>${aspectjweaver-version}</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/javax.annotation/javax.annotation-api -->
		<dependency>
			<groupId>javax.annotation</groupId>
			<artifactId>javax.annotation-api</artifactId>
			<version>${javax.annotation-version}</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>

		<!-- oracle jdbc 
		<dependency> 
			<groupId>com.oracle.jdbc</groupId>
			<artifactId>ojdbc8</artifactId> 
			<version>${com.oracle-version}</version>
		</dependency> -->
				
		<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-dbcp2 -->
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-dbcp2</artifactId>
			<version>${org.apache.commons-version}</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>${org.mybatis-version}</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>${org.mybatis-spring-version}</version>
		</dependency>

	</dependencies>
</project>
```