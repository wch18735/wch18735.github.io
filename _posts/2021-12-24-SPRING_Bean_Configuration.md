---
title: "[Spring] Bean 태그를 이용한 의존성 주입"
excerpt: "dependency injection by beans"
date: 2021-12-23
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - dependency injection
    - bean
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

<span style="font-size: 1.5em;">Bean configuration 내용 정리</span>

이번 포스팅에서 정리할 내요을 간단하게 요약해봤다.

- bean config xml file 사용법
- bean 태그 기본 속성
  - class : 객체를 생성하기 위해 사용할 클래스를 지정
  - id : Bean 객체를 가져오기 위해 사용하는 이름을 지정
  - lazy-init : 싱글톤인 경우 xml 로딩할 때 객체 생성 여부 설정. True 옵션일 때 로딩 시 객체를 생성하지 않고 `getBean` 메소드로 호출할 때 객체를 생성
  - scope : 객체 범위를 지정함
    - singleton : 객체를 하나만 생성해서 사용
	- prototype : 객체를 가져올 때 마다 생성해서 사용
- ApplicationContext
- bean 객체 property 활용
- bean 객체 생성자 사용법 (index, type)
- Collection 주입

<br/>
<span style="font-size: 1.5em;">bean.xml 파일</span>

XML 파일의 *bean 태그* 는 소스코드 외부에서 객체를 생성해 주입할 수 있도록 흐름을 제어하는 기능을 한다. `<bean/>` 태그를 사용하기 위해서 문서에 아래 내용을 삽입하고 시작한다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

<beans/>
```

<br/>
<span style="font-size: 1.5em;">XML namespace</span>

*xmlns* 란 XML 네임스페이스(namespace)를 뜻한다. XML 네임스페이스는 요소의 이름과 속성의 이름을 하나의 그룹으로 묶어 동일한 이름에 대해 발생하는 충돌을 회피할 수 있도록 해준다. 식별은 XML 네임스페이스의 URI(Uniform Resource Identifiers)를 사용한다.

간단한 예시로 `resource` 라는 이름의 태그가 두 XML 파일 내부에 존재하는 상황을 가정하자. 서로 두 XML 파일을 네임스페이스를 이용해 구분하려면 다음과 같이 나타낼 수 있다.

```xml
<a:resuorce xmlns:a="URI"/>
<b:resource xmlns:b="URI"/>
```

만약 *URI* 가 너무 길어지면 다음과 같은 방법도 가능하다. 아래 예시는 같은 `body` 태그에 대한 접두사를 서로 다르게 하여 구분하는 방법이다.

```xml
<root
    xmlns:a="https://www.w3.org/TR/html5/"
    xmlns:b="http://codingsam.com/xml/physical/">
    <a:body>
        <a:h1>html에서의 제목</a:h1>
        <a:p>html에서의 단락</a:p>
    </a:body>
    <b:body>
        <b:arm>70</b:arm>
        <b:leg>110</b:leg>
    </b:body>
</root>
```

끝으로 `xsi` 는 보다시피 *XMLSchema Instance* 를 타나내는 것이다. 여하튼 다시 Spring Dependency Injection 으로 돌아오자.

<br/>
<span style="font-size: 1.5em;">객체 생성 예제</span>

자바 소스코드 내부가 아닌 설정 파일에서 객체를 생성하고 전달하는 예제를 정리해보자. 먼저 `spring.di.entity` 라는 패키지를 만들고 간단한 *Entity* 클래스를 생성한다. *Eclipse IDE* 에서는 *Entity* 내부 Attribute 들만 선언한 다음 `Alt + Shift + S` 단축키로 편리하게 *Getter & Setter* 생성이 가능하다.

```java
// SimpleEntity.java
package spring.di.entity;

public class SimpleEntity {
	private String name;
	private int number;
	
	public SimpleEntity() {}
	
	public SimpleEntity(String name, int number) {
		this.name = name;
		this.number = number;
	}

	public String getName() {
		return name;
	}

	public int getNumber() {
		return number;
	}

	public void setNumber(int number) {
		this.number = number;
	}

	public void setName(String name) {
		this.name = name;
	}
}
```

이후 원하는 위치에 XML을 작성해준다. `id` 는 `simple` 이고 `class` 는 `spring.di.entity.SimpleEntity` 를 참조하는 `bean` 을 이 파일에서 생성할 것이다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<!-- 생성부 -->
	<bean id="simple" class="spring.di.entity.SimpleEntity"/>
</beans>
```

이렇게 소스코드가 아닌 XML 파일에서 생성한 빈은 자바 소스코드 어디에서든 가져다 사용할 수 있다. 이 때, <u>ApplicationContext 라는 인터페이스를 사용한다.</u> 이 Interface 는 XML 전달 방법에 따라 다음과 같이 네 가지로 나눠진다. 이름 앞에 붙은 클래스, 파일, XmlWeb, Annotation 등에서 전달 방법을 예상해 볼 수 있다.

- ClassPathXmlApplicationContext
- FileSystemXmlApplicationContext
- XmlWebApplicationContext
- AnnotationConfigApplicationContext

위에서 말 한 네 가지 클래스 중 ClassPathXmlApplicationContext 를 이용해 빈을 받아오는 한 가지 예제가 아래와 같다. 정의한 context 에서 `getBean` 메소드에 불러오려는 빈의 `id`를 전달해 전달하는 것을 확인할 수 있다. 이 때, `(SimpleEntity)` 로 타입 캐스팅을 해주는데, `getBean('simple', SimpleEntity.class)` 와 같이 뒤를 수정하면 타입 캐스팅을 따로 지정해주지 않아도 된다.

```java
package spring.di;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import spring.di.entity.SimpleEntity;

public class Program {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("spring/di/setting.xml");
		SimpleEntity se = (SimpleEntity) context.getBean("simple");
	}
}
```

이렇게 작성된 코드에는 아직까지 문제가 하나 있다. 현재 받아온 객체는 초기화가 되어있지 않다. 이를 어떻게 해결할 수 있을까. 바로 클래스 내에 정의했던 setter 메소드를 사용한다. `<bean>` 태그 안에 `<property>` 태그를 사용해 빈이 가지는 여러 속성들을 사용할 수 있는데 그 중 하나인 setter 메소드를 사용하는 예제가 아래와 같다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="simple" class="spring.di.entity.SimpleEntity">
		<property name="name" value="simple name"></property>
	</bean>
</beans>
```

이렇게 작성하면 소스코드 외부에서 미리 초기화 한 빈을 전달할 수 있다. 실제로 아래 코드를 컴파일해서 실행시켜보면 `simple name` 이라는 문자열이 출력되는 것을 볼 수 있다.

지금까지는 클래스 내에 정의된 setter 메소드를 이용해 값을 설정했다. 그러면 XML 파일 내에서 생성자를 호출해 객체를 생성하고 전달할 수 있을까? 역시 가능하다. `<constructor-arg>` 태그를 사용한다. *Constructor argument* 라는 뜻이다. 아래는 0 번부터 시작하는 인자들의 `index` 를 지정해 값을 전달하는 방법이다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="simple" class="spring.di.entity.SimpleEntity">
		<constructor-arg index="1" value="1"></constructor-arg>
		<constructor-arg index="0" value="simple name"></constructor-arg>
	</bean>
</beans>
```

또는 아래와 같이 매개변수로 지정된 이름을 지정해 줄 수 있다. 개발자에게 더 편한 방법으로 사용할 수 있으나 나는 개인적으로 아래 방법을 더 선호한다. 매개변수 명에서 예측할 수 있는 많은 정보를 index 만으로는 알 수 없기 때문이다. 혹시나 빈이 수정되는 경우 index 역시 수정해야 할 가능성이 큰 것도 이유가 될 수 있겠다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="simple" class="spring.di.entity.SimpleEntity">
		<constructor-arg name="number" value="1"></constructor-arg>
		<constructor-arg name="name" value="simple name"></constructor-arg>
	</bean>
</beans>
```

자료형이 다른 경우로 생성자 오버로드가 된 경우도 고려해볼 만한 문제다. 다행히 자료형을 지정하는 옵션이 존재한다. `type="float"` 과 같이 자료형을 지정해 어떤 생성자를 실행시킬 것인지 결정할 수 있다.

여기까지 *property* 또는 *constructor-arg* 를 활용해 만들어진 빈에 속성을 부여하는 방법을 알아봤다. 마지막으로 새로운 처리기를 불러와 코드를 간단하게 해보자. 여러 줄에 적었던 코드를 한 줄로 끝낼 수 있어 가독성을 높인다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="simple" class="spring.di.entity.SimpleEntity" p:name="simple name" p:number="1"/>
</beans>
```

<br/>
<span style="font-size: 1.5em;">XML 에서 Collection 생성</span>

콜렉션 생성을 다뤄보자. SimpleEntity 배열을 외부에서 만들어 전달해본다. 생성되는 콜렉션 객체가 사용자가 설정하는 초기 값을 가지는지 확인하기 위해 아래와 같이 코드를 작성한다.

```java
package spring.di;

import java.util.List;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import spring.di.entity.SimpleEntity;

public class Program {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("spring/di/setting.xml");
		List<SimpleEntity> ses = (List<SimpleEntity>)context.getBean("simple entities");
		
		for(SimpleEntity se : ses)
			System.out.println(se.getName());
	}
}
```

그리고 XML 설정 파일(setting.xml) 을 다음과 같이 수정한 뒤 실행해보자. 잘 동작하는 것이 보인다. 여기서 보면 기존 설정 파일에 남아있는 레퍼런스를 그대로 불러와 사용하는 `<ref bean="prevEntity"/>` 를 볼 수 있다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="prevEntity" class="spring.di.entity.SimpleEntity" p:name="simple01" p:number="1"></bean>
	<bean id="simple entities" class="java.util.ArrayList">
		<constructor-arg>
			<list>
				<bean class="spring.di.entity.SimpleEntity" p:name="simple02" p:number="2"></bean>
				<ref bean="prevEntity"/>
			</list>
		</constructor-arg>
	</bean>
</beans>
```

간단하게 생각하면 `constructor-arg` 태그는 생성자의 괄호와 같다. 이렇게 간단하게 매칭시켜 놓으면 까먹지 않는다. 내부 `<list>` 태그로 컬렉션을 생성해 전달한다. 이 역시 더 간단하게 작성될 수 있는데, 방법은 아래처럼 `util` 네임스페이스를 불러오는 것이다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:util="http://www.springframework.org/schema/util"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans 
    http://www.springframework.org/schema/beans/spring-beans-3.2.xsd 
    http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-3.2.xsd">

	<bean id="prevEntity" class="spring.di.entity.SimpleEntity" p:name="simple01" p:number="1"></bean>
	<util:list id="simple entities" list-class="java.util.ArrayList">
		<bean class="spring.di.entity.SimpleEntity"></bean>
		<ref bean="prevEntity"></ref>
	</util:list>
</beans>
```

util 처리기는 constant, list, map, set, properties, property-path 등을 지원한다. 검색해보면 다양한 예시를 살펴볼 수 있다. 다음은 어노테이션을 이용해 의존성을 주입하는 방법을 정리해 볼 것이다.