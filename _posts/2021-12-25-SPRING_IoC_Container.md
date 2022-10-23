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

- IoC Container는 IoC를 관장하는 컨테이너이다.
- IoC (Inversion of Contrl)
	- 제어 역전을 뜻하는 것으로 Framework이 제공하는 성질 중 하나이다
	- 일반적으로 프로그램을 작성할 때 프로그램이 흘러가는 흐름이나 객체 제어권은 개발자에게 있으나, 이를 프레임 워크에 위임하는 것을 뜻함
	- 프레임워크 사용자(개발자)는 정해진 틀(Frame) 안에서 동작이나 방법들을 상세화하는 방법으로 소프트웨어를 구축하게 된다
- Spring Framework 에서는 xml 또는 자바 어노테이션을 이용해 이를 지원한다


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
- 사용 형태
	- XmlBeanFactory

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

### ApplicationContext

- 클래스를 통해 객체를 생성하고 객체의 주소값을 반환하는 방식으로 전달한다.
- 상속 등 객체 간의 관계를 형성하고 관리한다.
- 국제화 지원 등 문자열에 관련된 다양한 기능을 제공한다.
- 리스너로 등록된 Bean에 이벤트를 발생시킬 수 있다.
- Bean 에 관련된 설정을 위한 xml 파일은 즉시 로딩하면서 객체를 미리 생성해 가지고 있다.
- 사용 형태
	- ClassPathXmlApplicationContext
	- FileSystemXmlApplicationContext
	- XmlWebApplicationContext
	- AnnotationConfigApplicationContext

다음은 ApplicationContext 활용 예제다. 앞서 보았던 BeanFactory 예제 코드와 비슷하지만 여러 가지 옵션이 가능하다는 점에서 차이를 보인다. 로드와 동시에 객체를 생성하며, 같은 **id**로 클래스를 호출해도 다른 객체를 전닫받을 수 있도록 조절할 수 있다. 

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

위 코드를 실행하기만해도 생성자가 호출되는 것을 확인할 수 있다. 이를 BeanFactory 처럼 객체를 호출할 때 생성자가 호출되도록 바꾸고 싶다면 `lazy-init="true"` 옵션을 삽입해보자. 이외에도 다양한 bean 태그 기본 속성들과 이를 응용하는 방법들이 있다. 

ctx로부터 여러 번 bean을 가져오는 `getBean` 을 호출하면 항상 같은 주소가 반환된다. 이를 바꿔주기 위해 xml 파일에 `<bean id='testBean' class='com.springpractice.beans.TestBean' scope="prototype"></bean>` 다음과 같이 선언해보자. `scope` 를 바꿔주는 것이다. 그리고 아래 코드를 실행해보면 서로 다른 주소 값이 반환되는 것을 확인할 수 있다.

```java
public class MainClass {

	public static void main(String[] args) {
		test3();
	}

	// ApplicationContext - 패키지 내부
	public static void test3() {
		ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("com/springpractice/config/beans.xml");
		TestBean tb1 = ctx.getBean("testBean", TestBean.class);
		TestBean tb2 = ctx.getBean("testBean", TestBean.class);
		
		System.out.printf("tb1 : %s\n", tb1);
		System.out.printf("tb2 : %s\n", tb2);
		ctx.close();
	}
}

/*
02:01:41.662 [main] DEBUG org.springframework.context.support.ClassPathXmlApplicationContext - Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@f5f2bb7
02:01:41.952 [main] DEBUG org.springframework.beans.factory.xml.XmlBeanDefinitionReader - Loaded 2 bean definitions from class path resource [com/springpractice/config/beans.xml]
02:01:41.993 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'com.springpractice.beans.TestBean#0'
생성 완료
생성 완료
tb1 : com.springpractice.beans.TestBean@56de5251
tb2 : com.springpractice.beans.TestBean@419c5f1a
02:01:42.047 [main] DEBUG org.springframework.context.support.ClassPathXmlApplicationContext - Closing org.springframework.context.support.ClassPathXmlApplicationContext@f5f2bb7, started on Tue Jan 04 02:01:41 KST 2022
*/
```