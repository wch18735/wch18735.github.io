---
title: "[Spring] 자동 주입 (Auto Injection)"
excerpt: "auto injection by autowire"
date: 2022-01-06
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - dependency injection
    - bean
    - autowire
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 자동 주입

Bean 을 정의할 때 `<constructor-arg>` 또는 `<property name=" " ref=" ">` 를 통해 직접 주입하는 방법 외에도 Spring 에서는 자동으로 객체를 주입할 수 있도록 지원한다. 각각 이름, 타입, 생성자를 이용하며 빈에 `autowire=" "` 옵션을 적용해 사용한다.

IoC와 Dependency Injection 개념이 많이 헷갈렸는데 이번 포스트를 정리하면서 정리가 됐다. IoC(Inversion of Control)는 소스코드 내에서 ApplicationContext를 이용해 외부에서 객체를 가져오는 작업을 말하고, Dependency Injection은 그 객체에 각종 초기 값들을 주입하는 과정을 말한다.

지금까지 정리된 내용을 나중에도 알기 쉽게 아래와 같이 용어를 정리했다.

- 전달 : Java 코드 상에서 ApplicationContext 로부터 전달받는 행위
- 주입 : xml 파일에서 전달하려는 객체에 의존성을 주입하는 행위

## 이름을 통한 주입 (byName)

빈을 참조변수에 담을 때 이용하는 autowire 중 byName 옵션은 빈 객체의 프로퍼티 이름과 정의된 빈의 이름이 같은 것을 찾아 자동으로 주입한다.

보통의 injection 은 아래와 같은 방법으로 수행한다. 이 과정을 `<autowire=''byName>` 옵션을 통해 간단하게 진행할 수 있다.

```xml
<!-- inject by normal way -->
<bean id="obj1" class="com.springpractice.beans.TestBean1">
    <property name="data1">
        <bean class="com.springpractice.beans.DataBean1"></bean>
    </property>
    <property name="data2" ref="data_bean1"></property>
</bean>
<bean id='data_bean1' class="com.springpractice.beans.DataBean1"></bean>
```

만약 TestBean1 객체가 변수 이름으로 data1, data2를 가지며 IoC 컨테이너(여기서는 beans.xml) 내부에 `<id='data1'>`, `<id='data2>` 로 지정된 빈이 존재할 때, 자동으로 둘은 매칭되어 해당 빈이 TestBean1 객체에 **주입**된다. 즉, 아래와 같이 코드를 작성하면 `TestBean1` 타입의 객체 `obj2` 는 자동으로 이름이 같은 두 빈을 주입받는다. 

```xml
<!-- inject by name -->
<bean id='obj2' class="com.springpractice.beans.TestBean1" autowire='byName'></bean>
<bean id='data1' class="com.springpractice.beans.DataBean1"></bean>
<bean id='data2' class="com.springpractice.beans.DataBean1"></bean>
```

## 타입을 통한 주입 (byType)

byType 옵션은 전달하려는 객체가 가진 변수들의 타입과 현재 컨테이너의 클래스 타입을 비교해 매칭되는 형태로 주입한다. 이 때, 동일한 타입의 클래스가 여러 개 존재하는 경우 에러가 발생한다.

TestBean2 클래스는 DataBean2 타입의 변수 2개를 가지고 있다. 이 경우 클래스가 가진 데이터 타입을 기준으로 아래와 같이 자동 주입할 수 있다.

```xml
<!-- inject by type -->
<bean id='obj3' class="com.springpractice.beans.TestBean2" autowire='byType'></bean>
<bean id='data_bean2' class='com.springpractice.beans.DataBean2' scope='prototype'></bean>
```

## 생성자를 통한 주입 (constructor)

constructor 옵션은 전달하려는 객체가 생성자를 가지고 있을 때, 컨테이너 상에서 해당 타입을 가진 빈을 자동으로 매핑하고 생성자를 호출한다. 이 때, `int` 나 `String` 같은 참조 변수가 아닌 값은 직접 지정해야한다.

```xml
<!-- inject by constructor autowire with variables -->
<bean id='obj7' class='com.springpractice.beans.TestBean3' autowire='constructor'>
    <constructor-arg value='123' type='int'></constructor-arg>
    <constructor-arg value='autowire="constructor" 참조변수가 아닌 값은 직접 지정해야 함' type='java.lang.String'></constructor-arg>
</bean>
```

## Default 설정

해당 autowire 설정은 아래와 같이 `beans` 태그 내부에 기본값으로 어떤 자동 주입 방식을 따를지 결정할 수 있다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
                    http://www.springframework.org/schema/beans/spring-beans.xsd"
default-autowire="byName">
</beans>
```