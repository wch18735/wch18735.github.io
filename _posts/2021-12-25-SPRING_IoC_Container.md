---
title: "[Spring] IoC Container"
excerpt: "inversion of control container"
date: 2022-01-01
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - dependency injection
    - bean
    - inversion of control
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# IoC 컨테이너

- IoC (Inversion of Contrl) : 제어 역전
- 일반적으로 프로그램을 작성할 때 프로그램이 흘러가는 흐름이나 생성되는 객체에 대한 제어권을 개발자가 가지는 것과 달리 프레임 워크가 가지는 것을 의미함.
- 개발자가 코드의 흐름이나 객체 생성에 관련된 코드를 소스에 직접 작성하는 것이 아닌 프레임워크가 사용하는 파일에 작성하면 이를 토대로 프레임워크가 객체를 생성하여 반환하고 코드가 동작하는 순서를 결정하게 된다는 것.
- Spring Framework 에서는 xml 또는 자바 어노테이션을 이용해 이를 지원한다.

## POJO Class

- POJO (Plain Old Java Object) : 자바 모델이나 기능, 프레임워크 등에 따르지 않고 홀로 독립적이며 단순한 기능만 가진 객체들을 의미한다
- 자바에서는 이러한 객체들을 Bean 이라고 부른다.
- 학생 정보를 관리하거나 학교 정보들을 관리하는 객체들이 예시가 된다.
- POPO(PHP), POCO(.NET Framework), PODS(C++), POD(Perl) 등이 있다.

이러한 Java POJO Classes 들은 개발자가 설정한 Metadata 를 이용해 Spring Container 에서 생성된다.

## IoC 컨테이너의 종류

IoC 컨테이너의 종류는 BeanFactory 와 ApplicationContext 두 가지로 나뉜다. 각각은 다음과 같은 특징을 가진다.

### BeanFactory

- 클래스를 통해 객체를 생성하고 객체의 주소값을 반환하는 방식으로 전달한다.
- 상속 등 객체 간의 관계를 형성하고 관리한다.
- Bean에 관련된 설정을 위한 xml 파일은 즉시 로딩되지만 객체는 개발자가 요구할 때 생성한다.
- 옛날에 쓰이던 기술로 현재는 많이 사용되지 않음

**사용 형태**

- XmlBeanFactory

### ApplicationContext

- 클래스를 통해 객체를 생성하고 객체의 주소값을 반환하는 방식으로 전달한다.
- 상속 등 객체 간의 관계를 형성하고 관리한다.
- 국제화 지원 등 문자열에 관련된 다양한 기능을 제공한다.
- 리스너로 등록된 Bean에 이벤트를 발생시킬 수 있다.
- Bean 에 관련된 설정을 위한 xml 파일은 즉시 로딩하면서 객체를 미리 생성해 가지고 있다.

**사용 형태**

- ClassPathXmlApplicationContext
- FileSystemXmlApplicationContext
- XmlWebApplicationContext
- AnnotationConfigApplicationContext

## 예제 코드

<span style="font-size: 1.5em">BeanFactory 활용 예제</span>

먼저 BeanFactory 활용 예제를 알아보자. 다음과 같이 TestBean 클래스를 작성하고 간단한 생성자를 만든다.

```java
package com.springpractice.beans;

public class TestBean {
	public TestBean(){
		System.out.println("생성 완료");
	}
}
```

이후 MainClass 파일에서 BeanFactory 를 생성해본다. 

위에서도 설명했지만 BeanFactory는 개발자가 요구할 때 객체를 생성하기 때문에 xml 파일이 로드되어도 객체를 생성하지 않으면 생성자 동작하지 않는다. 아래 코드에서 주석 부분을 포함 또는 배제해가며 그 차이를 확인할 수 있다.

또 한 가지 재미있는 점으로 동일한 `id` 값을 사용해 TestBean 을 호출해도 생성자는 한 번만 호출된다는 것이다. 즉, Container 는 기존에 생성한 Class 주소 한 가지만을 포함하고 있다.

```java
package com.springpratice.main;

import org.springframework.beans.factory.xml.XmlBeanFactory;
import org.springframework.core.io.ClassPathResource;

import com.springpractice.beans.HelloWorld;
import com.springpractice.beans.TestBean;

public class MainClass {

	public static void main(String[] args) {
		test1();
	}

	// BeanFactory - 패키지 내부
	public static void test1() {
		ClassPathResource res = new ClassPathResource("com/springpractice/config/beans.xml");
		XmlBeanFactory factory = new XmlBeanFactory(res);

		/* ******************************************************** */
		TestBean t1 = factory.getBean("t1", TestBean.class);
		TestBean t2 = factory.getBean("t1", TestBean.class);
		/* ******************************************************** */
	}
}
```

패키지 외부에 xml 파일이 있을 때 사용하는 방법으로 FileSystemResource 가 있다. 기존 코드와 거의 동일하며 xml 파일이 존재하는 절대적 위치를 넘겨주는 차이가 있다.

```java
package com.springpratice.main;

import org.springframework.beans.factory.xml.XmlBeanFactory;
import org.springframework.core.io.FileSystemResource;

import com.springpractice.beans.HelloWorld;
import com.springpractice.beans.TestBean;

public class MainClass {

	public static void main(String[] args) {
		test2();
	}

	// BeanFactory - 패키지 외부
	public static void test2() {
		FileSystemResource res = new FileSystemResource("beans.xml");
		XmlBeanFactory factory = new XmlBeanFactory(res);

		/* ******************************************************** */
		TestBean t1 = factory.getBean("t1", TestBean.class);
		TestBean t2 = factory.getBean("t1", TestBean.class);
		/* ******************************************************** */
	}
}
```

<span style="font-size: 1.5em">ApplicationContext 활용 예제</span>

다음은 ApplicationContext 활용 예제다. 위 코드와 비슷하지만 여러 가지 옵션이 가능하다는 점이 다르다. 로드와 동시에 객체를 생성한다는 점과 같은 `id` 로 클래스를 호출해도 다른 객체를 전닫받을 수 있도록 조절할 수 있다.

```java
package com.springpratice.main;

import org.springframework.beans.factory.xml.XmlBeanFactory;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.core.io.ClassPathResource;
import org.springframework.core.io.FileSystemResource;

import com.springpractice.beans.HelloWorld;
import com.springpractice.beans.TestBean;

public class MainClass {

	public static void main(String[] args) {
		test3();
	}

	// ApplicationContext - 패키지 내부
	public static void test3() {
		ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("com/springpractice/config/beans.xml");
		ctx.close();
	}
}
```

더 자세한 예시는 [여기](https://wch18735.github.io/spring/SPRING_Dependency_Injection/)에서 확인할 수 있다.