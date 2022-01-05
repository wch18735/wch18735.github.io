---
title: "[Spring] Collection 주입"
excerpt: "collection type injection by beans"
date: 2022-01-06
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - dependency injection
    - bean
    - collections
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Collection Injection

Bean 을 정의할 때 주입해야 하는 멤버가 컬렉션인 경우가 있다. 이러한 컬렉션을 초기에 설정하는 방법이 존재하며 종류로는 List, Map, Set, Property 가 존재한다. 아래는 활용법과 예제를 간단하게 정리해봤다. Setter 를 정의한 뒤 bean.xml 에서 어떻게 활용할 수 있는지 필요할 때마다 찾아볼 수 있도록 했다.

```java
// TestBean.java
package com.springpractice.beans;

import java.util.List;
import java.util.Map;
import java.util.Set;

public class TestBean {
	private List<String> slist;
	private List<Integer> ilist;
	private List<DataBean> dlist;
	
	private Set<String> sset;
	private Set<Integer> iset;
	private Set<DataBean> dset;
	
	private Map<String, Object> omap;
	
	public Map<String, Object> getOmap() {
		return omap;
	}

	public void setOmap(Map<String, Object> omap) {
		this.omap = omap;
	}

	public Set<String> getSset() {
		return sset;
	}

	public void setSset(Set<String> sset) {
		this.sset = sset;
	}

	public Set<Integer> getIset() {
		return iset;
	}

	public void setIset(Set<Integer> iset) {
		this.iset = iset;
	}

	public Set<DataBean> getDset() {
		return dset;
	}

	public void setDset(Set<DataBean> dset) {
		this.dset = dset;
	}

	public List<DataBean> getDlist() {
		return dlist;
	}

	public void setDlist(List<DataBean> dlist) {
		this.dlist = dlist;
	}

	public List<Integer> getIlist() {
		return ilist;
	}

	public void setIlist(List<Integer> ilist) {
		this.ilist = ilist;
	}

	public List<String> getSlist() {
		return slist;
	}

	public void setSlist(List<String> slist) {
		this.slist = slist;
	}
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
						http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="t1" class="com.springpractice.beans.TestBean">
		<!-- Generic이 String인 List -->
		<property name="slist">
			<list>
				<value>문자열을</value>
				<value>담아</value>
				<value>전달</value>
			</list>
		</property>
		
		<!-- Generic이 Integer인 List -->
		<property name="ilist">
			<list>
				<value type="int">100</value>
				<value type="int">200</value>
				<value type="int">300</value>
			</list>
		</property>
		
		<!-- Generic이 ClassType인 List -->
		<property name="dlist">
			<list>
				<bean class="com.springpractice.beans.DataBean"></bean>
				<bean class="com.springpractice.beans.DataBean"></bean>
				<ref bean="data_bean"/>
			</list>
		</property>
		
		<!-- Generic이 String인 Set -->
		<property name="sset">
			<set>
				<value>문자열을</value>
				<value>담아</value>
				<value>전달</value>
			</set>
		</property>
		
		<!-- Generic이 Integer인 Set -->
		<property name="iset">
			<set>
				<value type="int">100</value>
				<value type="int">100</value>
				<value type="int">300</value>
			</set>
		</property>
		
		<!-- Generic이 ClassType인 Set -->
		<property name="dset">
			<set>
				<bean class="com.springpractice.beans.DataBean"></bean>
				<bean class="com.springpractice.beans.DataBean"></bean>
				<ref bean="data_bean"/>
				<ref bean="data_bean"/>
				<ref bean="data_bean"/>
			</set>
		</property>
		
		<!-- Generic이 Object인 Map -->
		<property name="omap">
			<map>
				<entry key="string" value="문자열"></entry>
				<entry key="integer" value="100" value-type="int"></entry>
				<entry key="class">
					<bean class="com.springpractice.beans.DataBean"></bean>
				</entry>
				<entry key="class2" value-ref="data_bean"></entry>
				<entry key="list">
					<list>
						<value>맵 안에</value>
						<value>문자열</value>
						<value>저장</value>
					</list>
				</entry>
			</map>
		</property>
	</bean>
	
	<!-- 기존에 만드는 Bean -->
	<bean id="data_bean" class="com.springpractice.beans.DataBean" scope="prototype"></bean>
	
</beans>
```

```java
// MainClass
package com.springpratice.main;

import java.util.List;
import java.util.Map;
import java.util.Set;

import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.springpractice.beans.DataBean;
import com.springpractice.beans.TestBean;

public class MainClass {

	public static void main(String[] args) {
		test();
	}

	public static void test() {
		ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("com/springpractice/config/beans.xml");
		TestBean t1 = ctx.getBean("t1", TestBean.class);
		
		List<String> slist = t1.getSlist();
		for(String str : slist)
			System.out.println(str);
		
		System.out.println("----------------------------");
		
		List<Integer> ilist = t1.getIlist();
		for(Integer i : ilist)
			System.out.println(i);
		
		System.out.println("----------------------------");
		
		List<DataBean> dlist = t1.getDlist();
		for(DataBean d : dlist)
			System.out.println(d);
		
		System.out.println("----------------------------");
		
		Set<String> sset = t1.getSset();
		for(String str : sset)
			System.out.println(str);
		
		System.out.println("----------------------------");
		
		Set<Integer> iset = t1.getIset();
		for(Integer i : iset)
			System.out.println(i);
		
		System.out.println("----------------------------");
		
		Set<DataBean> dset = t1.getDset();
		for(DataBean d : dset)
			System.out.println(d);
		
		System.out.println("----------------------------");
		
		Map<String, Object> omap = t1.getOmap();
		System.out.println(omap.get("string"));
		List<String> tmpList = (List<String>)omap.get("list");
		
		for(String str : tmpList)
			System.out.println(str);
		
		ctx.close();
	}
}
```

```sh
01:54:48.080 [main] DEBUG org.springframework.context.support.ClassPathXmlApplicationContext - Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@f5f2bb7
01:54:48.345 [main] DEBUG org.springframework.beans.factory.xml.XmlBeanDefinitionReader - Loaded 2 bean definitions from class path resource [com/springpractice/config/beans.xml]
01:54:48.395 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 't1'
문자열을
담아
전달
----------------------------
100
200
300
----------------------------
com.springpractice.beans.DataBean@689604d9
com.springpractice.beans.DataBean@18078bef
com.springpractice.beans.DataBean@799f10e1
----------------------------
문자열을
담아
전달
----------------------------
100
300
----------------------------
com.springpractice.beans.DataBean@7e07db1f
com.springpractice.beans.DataBean@1189dd52
com.springpractice.beans.DataBean@36bc55de
----------------------------
문자열
맵 안에
문자열
저장
01:54:48.522 [main] DEBUG org.springframework.context.support.ClassPathXmlApplicationContext - Closing org.springframework.context.support.ClassPathXmlApplicationContext@f5f2bb7, started on Thu Jan 06 01:54:48 KST 2022

```