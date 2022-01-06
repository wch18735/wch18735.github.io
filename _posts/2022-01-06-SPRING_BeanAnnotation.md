---
title: "[Spring] Bean Annotation"
excerpt: "bean annotation"
date: 2022-01-06
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - dependency injection
    - bean
    - annotation
author: 1FeS
comments: true
---

Bean 객체를 만들 때, Configuration.java 또는 bean.xml 파일이 아닌 Annotation을 이용해 객체 소스파일에서 Bean 설정을 완료하는 방식이다. 이로써 빈 객체를 등록하는 과정을 클래스를 작성하는 과정에 동시에 수행할 수 있다. Spring 핵심은 IoC 컨테이너에 빈을 생성하고 이들을 주입하는 방법이 점차 쉬운 방법으로 발전했다는 것이다.

어노테이션을 사용하기 위해 beans.xml 상단에 context 네임스페이스를 추가한다. 그리고 `<context:annotaion-config>` 태그를 통해 bean 객체 클래스에 설정된 어노테이션을 사용할 수 있도록 명시한다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
						http://www.springframework.org/schema/beans/spring-beans.xsd
						http://www.springframework.org/schema/context
						http://www.springframework.org/schema/context/spring-context.xsd">
						
	<!-- bean 클래스에 설정되어 있는 어노테이션을 이용함 -->
	<context:annotation-config/>
	
</beans>
```

## @Required

`@Required` 어노테이션은 빈 객체 내부에 필수적으로 주입되어야 할 변수를 다룬다. 그러나 `@Required`는 Spring 5.1 이상부터는 아무런 일도 하지 않는다. 5.1 이상 버전에서는 deprecated 되는 것을 확인할 수 있으며 빈 객체는 생성자에서 모든 값을 주입받도록 권장한다. 결론적으로 5.1 버전부터는 아무일도 하지 않는다.

## @Autowired

`@Autowired` 어노테이션은 클래스 타입을 통한 자동 주입을 지원한다. Bean 객체의 Setter 위에 `@Autowired` 를 지정한다. Setter 로 주입할 수 있는 객체 타입을 IoC 컨테이너에서 찾아 자동으로 주입한다. 아래와 같이 객체를 설계하는 과정에서 어노테이션을 부여해 간단하게 설정할 수 있다.

```java
public class TestBean1 {
	private int data1;
	private DataBean1 data2;
	
	public int getData1() {
		return data1;
	}

	public DataBean1 getData2() {
		return data2;
	}
	
	@Autowired
	public void setData2(DataBean1 data2) {
		this.data2 = data2;
	}

	public void setData1(int data1) {
		this.data1 = data1;
	}
}
```

더 편리한 점은 `@Autowired` 어노테이션을 변수 위에 선언하는 것만으로도 Setter 메서드 주입을 대체할 수 있다는 것이다. 아래 코드 역시 동일하게 동작하는 것을 확인할 수 있다. 참고로, 주입할 빈이 IoC 컨테이너에 존재하지 않을 때, 오류를 발생시키지 않는 방법이 있다. 원하는 빈 객체가 존재하지 않아 초기값 그대로 전달하기 위해서는 `@Autowired(required=false)` 어노테이션을 선언한다.

```java
package com.springpractice.beans;

import org.springframework.beans.factory.annotation.Autowired;

public class TestBean1 {
	private int data1;
	@Autowired
	private DataBean1 data2;
	
	public int getData1() {
		return data1;
	}

	public DataBean1 getData2() {
		return data2;
	}

	public void setData1(int data1) {
		this.data1 = data1;
	}
}
```

## Qualifier

`@Qualifier` 어노테이션은 자동 주입할 Bean 객체가 여러 개 존재하는 경우 사용한다. `byName` 옵션과 비슷하다고 느끼면 될 것 같다. 예를 들어서 DataBean1 타입 빈이 IoC 컨테이너에 여러 개 등록되어 있을 때, `id`를 통한 주입처럼 식별자에 따라 주입할 빈을 달리한다. 아래는 앞서 설명한 경우를 설명하는 경우다. 현재 IoC 컨테이너에 다음과 같은 빈 객체들이 등록된 경우를 생각해보자.

```java
// BeanConfigClass.java
package com.springpractice.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.springpractice.beans.DataBean1;
import com.springpractice.beans.TestBean1;

@Configuration
public class BeanConfigClass {
	
	@Bean(name="tb1")
	public TestBean1 bean1() {
		return new TestBean1();
	}
	
	@Bean(name="obj1")
	public DataBean1 dbean1() {
		return new DataBean1();
	}
	
	@Bean(name="obj2")
	public DataBean1 dbean2() {
		return new DataBean1();
	}
}
```

DataBean1 타입 클래스가 2개 등록된 상태에서 TestBean1 빈 객체에 둘 중 하나를 주입하려고 한다. 이 때, `@Qulifier`를 사용해 개발자가 원하는 빈을 식별해 값을 주입할 수 있다.

```java
package com.springpractice.beans;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;

public class TestBean1 {
	private int data1;
	@Autowired
	@Qualifier("obj1")
	private DataBean1 data2;
	
	public int getData1() {
		return data1;
	}

	public DataBean1 getData2() {
		return data2;
	}

	public void setData1(int data1) {
		this.data1 = data1;
	}
}
```

## 생성자 주입

생성자에 주입 시 참조변수 타입 변수들은 자동으로 바인딩되어 주입된다. 그러나 기본 자료형 및 문자열 값들은 직접 주입 값을 설정해야하는데, 이 때 `@Value` 어노테이션을 사용한다. 아래는 해당 어노테이션이 사용되는 예시다. 그런데 이렇게 동작시켜보면 주입이 되질 않는다. 들어보니까 생성자를 통해 주입하는 방법은 Component를 사용할 때 더 배울 수 있다고 한다.

```java
package com.springpractice.beans;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.beans.factory.annotation.Value;

public class TestBean2 {
	private int data1;
	@Autowired
	@Qualifier("obj1")
	private DataBean1 data2;
	
	public TestBean2() {}
	
	public TestBean2(@Value("100") int data1, DataBean1 data2){
		this.data1 = data1;
		this.data2 = data2;
	}

	public int getData1() {
		return data1;
	}

	public void setData1(int data1) {
		this.data1 = data1;
	}

	public DataBean1 getData2() {
		return data2;
	}

	public void setData2(DataBean1 data2) {
		this.data2 = data2;
	}	
}
```