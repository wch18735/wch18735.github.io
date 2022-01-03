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
	- constructor
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

<span style="font-size: 1.5em;">Spring Bean 객체 생성</span>

이번 포스팅에서 배울 내용을 간단하게 정리해보면 아래와 같다.

- bean config xml file 사용법
- bean 태그
- ApplicationContext
- bean 객체 property 활용
- bean 객체 생성자 사용법 (index, type)
- Collection 주입

*setting.xml 파일* 의 *bean 태그* 는 소스코드 외부에서 객체를 생성해 주입할 수 있도록 흐름을 제어하는 기능을 한다. 이 포스트에는 간단한 활용법을 정리해 추후 개발에 참고할 수 있도록 내용을 정리했다. `<bean/>` 태그를 사용하기 위한 setting.xml 가장 상단에 들어가는 내용은 다음과 같다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
```

자주 보이는 *xmlns* 란 해당 문서를 위한 XML 네임스페이스(namespace)를 명시한다. 이러한 XML 네임스페이스는 요소의 이름과 속성의 이름을 하나의 그룹으로 묶어주어 이름에 대한 충돌을 해결한다. 

XML 네임스페이스는 URI(Uniform Resource Identifiers)로 식별된다. xmlns 속성은 해당 문서가 XHTML 문서일 경우 반드시 명시되어야 하며, HTML 4.01에서는 유효하지 않으며, HTML5에서는 선택 사항이다.

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

*setting.xml* 에서는 객체를 만들 수 있다. 이제 만드는 방법을 예제를 통해 알아보려고 한다. 그 전에 준비해야 될 것들이 몇 개 있는데 코드를 복붙한 다음 자기가 직접 예제를 만들어보면 이해가 더 빠를 것 같다.

`spring.di.entity` 라는 패키지 안에 간단한 *Entity* 를 만들어보자. 패키지는 사실 아무렇게나 만들어도 좋다. 자기가 원하는 패키지명과 구조를 가지면 된다.

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

참고로 *Eclipse IDE* 에서는 *Entity* 내부 Attribute 들만 선언한 다음 `Alt + Shift + S` 키를 눌러 *Getter & Setter* 생성이 가능하다. 나중가면 이게 더 편리하고 좋다.

이후 원하는 위치에 *setting.xml* 을 작성해준다. 나중에 절대경로로 받아올 것이기 때문에 어디에 둬도 좋지만 되도록 *src 폴더 안쪽* 으로 정하는 것이 좋을 것 같다. 생성한 세팅 문서에 아래와 같이 `<bean id="class" class="spring.di.entity.SimpleEntity"/>` 를 추가해보자.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="simple" class="spring.di.entity.SimpleEntity"/>
</beans>
```

보다시피 `bean` 을 만들었다. 이 `bean`을 가져다 써야되는데 이것도 편리하게 구현되어있다. ApplicationContext 라는 인터페이스를 사용한다. 이 Interface 는 네 가지로 구현되는데, XML 을 전달할 때 넘기는 방법에 따라 다음과 같이 나눠진다. XML 을 넘기는 방법이 이름 앞에 나타나있다.

- ClassPathXmlApplicationContext
- FileSystemXmlApplicationContext
- XmlWebApplicationContext
- AnnotationConfigApplicationContext

그러면 `main` 함수가 포함된 위치에서 기존에 작성한 빈을 꺼내는 코드를 작성해보자.

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

에러? 안 난다. 간단하다. 이전 파일에서 *id* 로 설정했던 이름으로 불러오는 작업이 성공했다. 참고로 `context.getBean(SimpleEntity.class)` 와 같이 클래스를 전달해 받아오는 방법도 있다. 이러면 Casting 작업이 없어지긴 한다.

하여튼 문제가 하나 있다. 지금 받아온 객체는 초기화가 안 되어있다. 어떻게 해결할 수 있을까. *setter* 가 있으니까 미리 사용해 볼 수 있을까? 당연히 가능하다. `<bean>` 태그 안에 `<property>` 태그를 사용해 빈이 가지는 여러 속성들을 정의할 수 있다.

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

지금까지는 클래스 내에 정의된 *setter* 메소드를 이용해 값을 설정했다. 그러면 XML 파일 내에서 생성자를 호출해 객체를 생성하고 전달할 수 있을까? 역시 가능하다. `<constructor-arg>` 태그를 사용한다. *Constructor argument* 라는 뜻이다.

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

이렇게 *0* 번부터 시작하는 `index` 를 이용해 값을 전달한다. 이것보다 더 편한 방법이 있다. 아예 기존에 인자로 지정된 입력들을 사용할 수 있다.

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

자료형이 다른 경우로 생성자 오버로드가 된 경우는 어떻게 처리할 수 있을까? 간단하게 `type="float"` 과 같이 자료형을 지정해 어떤 생성자를 실행시킬 것인지 결정할 수 있다.

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

진짜 마지막으로 콜렉션 생성을 다뤄보자. SimpleEntity 배열을 외부에서 만들어 전달해본다. 생성되는 콜렉션 객체가 사용자가 설정하는 초기 값을 가지는지 확인하기 위해 아래와 같이 코드를 작성한다.

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

util 처리기는 constant, list, map, set, properties, property-path 등을 지원한다. 검색해보면 다양한 예시를 살펴볼 수 있다. 다음은 어노테이션을 이용해 의존성을 주입하는 방법을 정리한다.