---
title: "[Spring] Spring Framework 기반 Oracle JDBC 사용 예제"
excerpt: "oracle jdbc practical example based on spring framework"
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

# Spring Framework JDBC

Spring Framework 는 JDBC 사용을 위해 JdbcTemplate 클래스를 제공한다. JdbcTemplate 클래스는 SQL 쿼리문을 손쉽게 구현할 수 있도록 구현되어있다.

jdbc 와 dbcp 라이브러리를 추가한다. Oracle을 제외한 다른 DBMS jdbc 파일들은 maven repository 에서 다운받을 수 있다. 그래서 나는 직접 다운받아 외부 라이브러리 추가해 사용했다. 끝으로 스프링 프레임워크를 사용하기 위한 Configuration 파일을 작성하고 데이터베이스 정보를 가진 빈을 등록한다.

```java
// BeanClassConfig.java
package com.springpractice.config;

import org.apache.commons.dbcp2.BasicDataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.core.JdbcTemplate;

@Configuration
@ComponentScan(basePackages = {"com.springpractice.config", "com.springpractice.db"})
public class BeanConfigClass {
	
	@Bean
	public BasicDataSource source() {
		BasicDataSource sc = new BasicDataSource();	
		sc.setDriverClassName("oracle.jdbc.OracleDriver");
		sc.setUrl("jdbc:oracle:thin:@localhost:1521/xepdb1");
		sc.setUsername("WCH18735");
		sc.setPassword("PASSWORD");
		return sc;
	}
	
	@Bean
	public JdbcTemplate db(BasicDataSource source) {
		JdbcTemplate db = new JdbcTemplate(source);
		return db;
	}
}

```

실습에 사용할 빈 클래스는 이전에 만들어뒀던 TestTable 엔티티를 모방해 만들었다. 객체를 설계하면서 `@Component` 어노테이션을 이용해 곧 바로 빈 설정 파일에 빈으로 등록할 수 있는게 굉장히 간편한 기능임을 알 수 있다. 빈 객체와 해당 객체를 관리하는 DAO, DAO에 주입할 Mapper 를 아래와 같이 작성해 추가했다. Mapper 클래스란 Select 문이 데이터를 가져올 때 어떤 컬럼 값을 Bean 어디에 주입할 것인지 결정해주는 클래스다. 해당 Mapper 클래스 내부에 사용자가 미리 규칙을 정해놓으면 조회한 행 정보에서 컬럼에 따라 값들을 자동으로 입력한다. 기존 ResultSet 과 달리 얻어온 행을 Bean 에 자동 주입한 후 이들을 리스트에 담아 전달한다.

```java
// TableMapper.java
package com.springpractice.db;

import java.sql.ResultSet;
import java.sql.SQLException;

import org.springframework.jdbc.core.RowMapper;
import org.springframework.stereotype.Component;

import com.springpractice.beans.TableBean;

@Component
public class TableMapper implements RowMapper<TableBean> {
	public TableBean mapRow(ResultSet rs, int rowNum) throws SQLException {
		// TODO Auto-generated method stub
		TableBean bean = new TableBean();
		
		// Mapper 내부에 어떤 Column 사용할지 정함
		bean.setId(rs.getInt("ID"));
		bean.setColor(rs.getString("COLLOR"));
		bean.setName(rs.getString("NAME"));
		
		return bean;
	}
}
```

```java
// TableDAO.java
package com.springpractice.db;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;

import com.springpractice.beans.TableBean;

@Component
public class TableDAO {
	@Autowired
	private JdbcTemplate db;
	
	// TableMapper 자동 주입
	@Autowired
	private TableMapper mapper;
	
	public void insert_data(TableBean bean) {
		String sql = "INSERT INTO TEST_TABLE (ID, NAME, COLLOR) VALUES (?, ?, ?)";
		db.update(sql, bean.getId(), bean.getName(), bean.getColor());
	}
	
	public List<TableBean> select_data() {
		String sql = "SELECT * FROM TEST_TABLE";
		List<TableBean> tbList = db.query(sql, mapper);
		
		return tbList;
	}
	
	public void update_data(TableBean bean) {
		String sql = "UPDATE TEST_TABLE SET NAME = ? WHERE COLLOR = ?";
		db.update(sql, bean.getName(), bean.getColor());
	}
	
	public void delete_data(TableBean bean) {
		String sql = "DELETE FROM TEST_TABLE WHERE ID = ?";
		db.update(sql, bean.getId());
	}
}

```

```java
// TableBean.java
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
}
```

단순 jdbc를 사용할 때에는 PreparedStatement 와 같이 경우에 따라 서로 다른 쿼리 생성 도구를 이용했다. 지금은 단순하게 update 만 사용하면 되니까 더욱 쉬워진 느낌이다. 이렇게 만든 DAO 를 설정 파일에 등록한 뒤 다음과 같은 CRUD 실습을 진행해 볼 수 있다.

```java
package com.springpractice.main;

import java.util.List;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContextExtensionsKt;

import com.springpractice.beans.TableBean;
import com.springpractice.config.BeanConfigClass;
import com.springpractice.db.TableDAO;

public class MainClass {
	public static void main(String[] args) {
		AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(BeanConfigClass.class);
		
		TableDAO dao = ctx.getBean(TableDAO.class);
		
		/* INSERT */
		TableBean insert_bean = new TableBean();
		insert_bean.setId(11);
		insert_bean.setColor("skyblue");
		insert_bean.setName("claire");
		dao.insert_data(insert_bean);
		
		/* UPDATE */
		TableBean update_bean = new TableBean();
		update_bean.setName("MICKEY");
		update_bean.setId(0);
		update_bean.setColor("skyblue");
		dao.update_data(update_bean);
		
		/* SELECT */
		List<TableBean> tbList = dao.select_data();
		for(TableBean tb : tbList) {
			System.out.printf("%d ", tb.getId());
			System.out.printf("%s ", tb.getName());
			System.out.printf("%s\n", tb.getColor());
		}
		
		/* DELETE */
		TableBean delete_bean = new TableBean();
		delete_bean.setId(11);
		dao.delete_data(delete_bean);
		
		ctx.close();
	}
}
```
