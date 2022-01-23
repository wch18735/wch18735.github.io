---
title: "[Spring] MyBatis 활용 예제"
excerpt: "Spring MyBatis practice example"
date: 2022-01-11
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

# MyBatis

MyBatis(마이바티스)는 Spring Framework 에서 제공하는 JDBC 라이브러리를 보다 쉽게 작업할 수 있도록 만든 라이브러리이다. Mapper 역할을 확장해 쿼리문 작성을 모두 Mapper 에서 처리할 수 있도록 지원한다.

이전에는 JDBC를 사용하다가 JdbcTemplate를 사용하면서 얻는 편리함을 잠깐 적었다. 이번에는 MyBatis 를 사용하는 간단한 예제를 적어볼까 한다.

다음과 같은 순서로 사용한다.

1) Bean 객체 정의
2) BeanConfigurationClass.java 파일에 
    2-1) BasicDataSource Bean 생성
    2-2) BasicDataSource Bean 주입해 SqlSessionFactory Bean 생성
    2-3) SqlSessionFactory Bean 주입해 MapperFactoryBean Bean 생성
3) MapperInterface 만들기 (`@Results`, `@Result`, `@Select`, `@Delete`, `@Update` 활용)

## Bean 객체 정의

테스트에 활용할 데이터 객체를 정의한다. TableBean 을 아래와 같이 선언해준다.

```java
package com.springpractice.beans;

import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

@Component
@Scope("prototype")
public class TableBean {
	private String color;
	private String name;
	private int id;
	
	public String getColor() {
		return color;
	}
	public void setColor(String color) {
		this.color = color;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	
	public void printData() {
		System.out.println(this.color);
		System.out.println(this.name);
		System.out.println(this.id);
	}
}
```

여기서 `printData()` 메소드는 여러 번 적기 귀찮아서 썼다.

## BeanConfigClass.java

```java
package com.springpractice.config;

import org.apache.commons.dbcp2.BasicDataSource;
import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.mapper.MapperFactoryBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.core.JdbcTemplate;

import com.springpractice.mapper.MapperInterface;

@Configuration
@ComponentScan(basePackages = {"com.springpractice.beans"})
public class BeanConfigClass {
	
	@Bean
	public BasicDataSource source() {
		BasicDataSource sc = new BasicDataSource();	
		sc.setDriverClassName("oracle.jdbc.OracleDriver");
		sc.setUrl("jdbc:oracle:thin:@localhost:1521/xepdb1");
		sc.setUsername("USERNAME");
		sc.setPassword("PASSWORD");
		return sc;
	}
	
	// SqlSessionFactory : jdbc 를 처리하는 객체
	@Bean
	public SqlSessionFactory factory(BasicDataSource source) throws Exception {
		SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
		factoryBean.setDataSource(source);
		SqlSessionFactory ssfactory = factoryBean.getObject();
		
		return ssfactory;
	}
	
	// Mapper (사용자가 원하는 만큼 정의할 수 있음)
	@Bean
	public MapperFactoryBean<MapperInterface> test_mapper(SqlSessionFactory factory) throws Exception {
		// Mapper 로 사용할 Interface 등록
		MapperFactoryBean<MapperInterface> factoryBean = new MapperFactoryBean<MapperInterface>(MapperInterface.class);
		factoryBean.setSqlSessionFactory(factory);
		return factoryBean;
	}
}
```

나는 자바 진영에서 사용하는 Factory라는 용어를 `Bean 객체를 생성해주는 일종의 틀`이라고 생각하고 있다. 예전에 어디서 봤던 것 같은데 `말을 만드려면 말 만드는 공장부터 지어줘야 합니다` 였나. 

어떤 객체를 생성하기 위해 세부 설정이나 필요한 전제 조건들이 갖춰진 공장이라고 이해하면 `SqlSessionFacotry`는 `SqlSession`을 뽑아내는 생성 객체라고 말할 수 있다.

BasicDataSource 정보를 가진 SqlSession Factory 가 MapperInterface 인터페이스를 반환하는 MapperFactoryBean 에 설정된 Bean 을 호출해 사용한다. 이 때, `MapperFactoryBean<T>` 에 설정해주는 Interface 에 따라 MapperFactoryBean 의 동작을 결정할 수 있다.

## MapperInterface 만들기

MapperInterface 에서 `SELECT`, `INSERT`, `UPDATE`, `DELETE` 동작들에 대한 쿼리를 지정할 수 있다. 변수는 `#{Bean 내부 variable 이름}`으로 지정할 수 있다.

```java
package com.springpractice.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Result;
import org.apache.ibatis.annotations.Results;
import org.apache.ibatis.annotations.Select;
import org.apache.ibatis.annotations.Update;

import com.springpractice.beans.TableBean;

public interface MapperInterface {
	@Results({
		@Result(column = "ID", property = "id"),
		@Result(column = "NAME", property = "name"),
		@Result(column = "COLOR", property = "color")
	})
	@Select("SELECT ID, NAME, COLOR FROM TEST_TABLE")
	List<TableBean> select_data();
	
	@Insert("INSERT INTO TEST_TABLE (ID, NAME, COLOR) VALUES (#{id}, #{name}, #{color})")
	void insert_data(TableBean bean);
	
	@Update("UPDATE TEST_TABLE SET NAME = #{name} WHERE ID = #{id}")
	void update_data(TableBean bean);
	
	@Delete("DELETE FROM TEST_TABLE WHERE NAME = #{name}")
	void delete_data(TableBean bean);
}
```

## TMI

sqldeveloper 열어놓고 작업할 때, 같은 테이블을 킨 상태로 `COMMIT;` 안 해주면 이클립스에서 하는 작업이 무한 대기에 빠지는 수가 있다.

오랜만에 테이블 다뤄보려니까 기억이 안 나서 인터넷 뒤져가면서 했다.

```sql
/* 특정 컬럼 삭제 */
DELETE FROM TEST_TABLE WHERE NAME = 'NOKCHA';

/* column 명 변경 */
ALTER TABLE TEST_TABLE RENAME COLUMN COLLOR TO COLOR;
```

이제 MyBatis 좀 써보는데 언제 JPA 배우고 익히지...