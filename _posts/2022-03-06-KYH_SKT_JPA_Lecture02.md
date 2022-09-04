---
title: "[JPA][토크ON세미나] JPA 프로그래밍 기본기 다지기 정리(2)"
excerpt: "[토크ON세미나] JPA 프로그래밍 기본기 다지기 정리(2)"
date: 2022-03-06
category:
    - jpa
tag:
    - jpa
    - h2 database
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# 데이터베이스 스키마 자동 생성

JPA는 JPA가 직접 데이터베이스 스키마 생성이 가능하다. DDL을 어플리케이션 실행 지점에 자동 생성이 가능하는 것이다. 따라서 데이터베이스에 명세에 따른 테이블 생성을 어플리케이션 레벨에서 수행 할 수 있게 된다.

이는 테이블 중심에서 객체 중심 데이터베이스 관리가 가능함을 뜻한다. 데이터베이스 방언을 활용하면 각 데이터베이스 종류에 맞는 적절한 DDL이 자동으로 생성된다. 그러나 이러한 DDL은 `개발 장비에서만 사용`하는 것을 권장하고 있다. 운영서버에서는 사용하지 않거나, **적절히 다듬은 후** 사용하는 것을 추천한다.

데이터베이스 스키마 생성에 사용하는 옵션으로 hibernate.hbm2ddl.auto 가 있다. 아래 `persistence.xml` 문서에 주석처리 된 부분이다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.2"
	xmlns="http://xmlns.jcp.org/xml/ns/persistence"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">
	<persistence-unit name="hello">
		<properties>
        <!-- ... -->
        
        <!-- 데이터베이스 스키마 자동 생성 -->
        <!--<property name="hibernate.hbm2ddl.auto" value="create" /> -->
		</properties>
	</persistence-unit>
</persistence>
```

여기에 세부 옵션이 존재하는데, 이에 따른 동작은 아래와 같다.

- **create:** 기존 테이블 삭제 후 다시 생성 (DROP + CREATE)
- **create-drop:** create 와 같으나 종료 시점에 테이블 DROP
- **update:** 변경분만 반영
- **validate:** 엔티티와 테이블이 정상 매핑되어는지만 확인
- **none:** 사용하지 않음

강사님은 이렇게 데이터베이스 스키마 자동 생성 할 때 주의점으로 `운영 장비에는 절대 create, create-drop, update 를 사용하지 않는다` 는 원칙을 권장한다. 지금은 더욱 보수적으로 바뀌어 개발에서조차 사용하지 않는 것을 권장하며 `alter` 구문 역시 개발자가 직접 작성하도록 정책을 수정하고 있다고 한다.

그치만.. 로컬에서 빠르게 추가하고 싶다면 위 옵션들이 편하다고 한다. 로컬에서 혼자 개발 할 때! 그럴 때만 사용하도록 권장하고 있다. 이를 실험해보기 위해 `create` 옵션을 주고 컬럼을 변경 후 실행해보면 DROP 을 수행하고, 테이블을 다시 생성하는 것을 볼 수 있다. 굉장히 위험...

<img src="/_img/2022-03-06/create_option.png">

## 매핑 어노테이션

이들은 모두 테이블과 객체 변수와의 관계들을 표현한 것이다. 

- `@Column`
- `@Temporal`
- `@Enumerated`
- `@Lob`
- `@Transient`

여기서 몇 가지 팁이 있는데, `@Enumerated` 내부에는 `EnumType.STRING` 을 쓰는 것을 권장하는 것이다. `EnumType.ORDINAL` 은 순서 값이 입력되기 때문에 Enum 타입이 바뀌게 되면 대응 값이 모두 바뀌는 경우가 생긴다.

```java
@Entity
@Getter
@Setter
public class Member {
	@Id
	private Long id;
	
	@Column(name = "USERNAME", nullable = true)
	private String name;
	
	private int age;
	
	@Temporal(TemporalType.TIMESTAMP)
	private Date regDate;
	
	@Enumerated(EnumType.STRING)
	private MemberType memberType;
}
```

### @Column

- 가장 많이 사용됨
- name: 필드와 매핑할 테이블 컬럼 이름
- insertable: 삽입 가능 여부 결정
- updateable : 값 업데이트 가능 여부 결정
- nullable: null 허용여부 결정, DDL 생성 시 사용
- unique: 유니크 제약 조건, DDL 생성 시 사용
- columnDefinition, length, precision, scale (DDL)

### @Lob

- CLOB, BLOB 매핑
- CLOB: String, char[], java.sql.CLOB
- BLOB: byte[], java.sql.BLOB
- `@Lob` 어노테이션을 넣으면 자동으로 매핑

### @Transient

- 필드는 매핑하지 않고 객체에서만 사용하는 값

## 식별자 매핑 방법

- `@Id`: Primary Key 값 직접 매핑
- **IDENTITY:** 데이터베이스에 위임
- **SEQUENCE:** 데이터베이스 시퀀스 오브젝트 사용
	- `@SequenceGenerator` 필요
- **TABLE:** 키 생성용 테이블 사용, 모든 데이터베이스 종류에 적용 가능
	- `@TableGenerator` 필요
- **AUTO:** 방언에 따라 자동 지정, 기본값

아래와 같이 `@GeneratedValue(strategy = GenerationType.AUTO)` 를 사용하고 실행해보면 H2 데이터베이스에서 자동으로 키 값이 생성되는 것을 확인할 수 있다. 직접 ID 값을 입력하지 않도록 sequence 가 생성된 것을 볼 수 있다.

```java
@Entity
@Getter
@Setter
public class Member {
	@Id @GeneratedValue(strategy = GenerationType.AUTO)
	private Long id;
	
	/* ... */
}
```

<br/>
<img src="/_img/2022-03-06/generated_value.png">
<br/>

### 권장되는 식별자 전략

기본 키 제약 조건은 다음 세 가지를 만족해야 한다. null 이 아니며, 유일하며, 변하면 안 되는 것이다. 미래까지 해당 조건을 만족하는 자연키는 찾기 어렵다. 그렇기 때문에 대리키(대체키)를 사용하는 것을 제안하고 있다. 

강의를 수행한 김영한님은 `Long 타입 + 대체키 + 키 생성전략을 사용하는 것`을 권장한다. 비즈니스랑 관계없는 것을 무조건 대체키로 사용하는 것.. 이를 경험으로부터 배운 것이라 한다.