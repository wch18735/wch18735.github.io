---
title: "[JPA][토크ON세미나] JPA 프로그래밍 기본기 다지기 정리(3)"
excerpt: "[토크ON세미나] JPA 프로그래밍 기본기 다지기 정리(3)"
date: 2022-03-13
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

# 연관관계 매핑

> 객체지향 설계의 목표는 자율적인 객체들의 협력 공동체를 만드는 것이다. <br/>- 조영호 (객체자향의 사실과 오해)

먼저 아래는 테이블에 맞게 객체를 설계하는 경우이다. 참조 대신에 외래키를 사용한다.

```java
@Entity
@Getter
@Setter
public class Member {
	@Id @GeneratedValue(strategy = GenerationType.AUTO)
	private Long id;
	
	@Column(name = "USERNAME", nullable = true, length=20)
	private String name;
	
	private int age;
	
	@Temporal(TemporalType.TIMESTAMP)
	private Date regDate;
	
	@Enumerated(EnumType.STRING)
	private MemberType memberType;
	
	@Column(name = "TEAM_ID")
	private Long teamId;
}

@Entity
public class Team {
	@Id @GeneratedValue
	private Long id;
	private String name;
}
```

이렇게 만든 두 객체에 대해 다음과 같이 Main 코드를 작성하고 실행해보자.

```java
public class Main {
	public static void main(String[] args) {
		EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
		EntityManager em = emf.createEntityManager();
		EntityTransaction tx = em.getTransaction();
		tx.begin();

		try {
			Team team = new Team();
			team.setName("TeamA");
			em.persist(team);
			
			Member member = new Member();
			member.setName("member1");
			member.setTeamId(team.getId());
			em.persist(member);
			
			tx.commit();
		} catch (Exception e) {
			tx.rollback();
		} finally {
			em.close();
		}
		emf.close();
	}
}
```

위 코드가 수행된 결과는 아래와 같다.

<img src="/_img/2022-03-13/relation_mapping1.png">

이렇게 되면 연관관계가 없기 때문에 멤버를 조회할 때 2번을 따로 저장해야 한다. 이러한 경우가 객체를 테이블에 맞추어 설계된 객체 스타일이라고 설명한다. 

테이블은 외래 키로 조인을 사용해 연관된 테이블을 찾는다. 예를들어 Team 에서 Member 를 찾을 수 있고, Member 에서 Team 을 찾을 수 있다. 그러나 객체 관계는 참조를 사용해 연관된 객체를 찾을 수 없다. 이 특징이 코드 레벨에서 verbose 함을 야기시키는 원인이 된다.

```java
public class Main {
	public static void main(String[] args) {

		/* ... */

		try {
			
			// Team 조회
			Member findMember = em.find(Member.class, member.getId());
			Long teamId = findMember.getTeamId();
			Team findTeam = em.find(Team.class, teamId);
			
			tx.commit();
		} 

        /* ... */
	}
}
```

## 단방향 매핑

객체의 참조와 테이블의 외래 키를 매핑한다. 즉, 객체 다이어그램 상에서 참조가 가능하도록 만드는 것이다. 물론 단방향이기 때문에 한 쪽에서만 가능하다.

```java
@Entity
@Getter
@Setter
public class Member {
	@Id @GeneratedValue(strategy = GenerationType.AUTO)
	private Long id;
	
	@Column(name = "USERNAME", nullable = true, length=20)
	private String name;
	
	private int age;
	
	@Temporal(TemporalType.TIMESTAMP)
	private Date regDate;
	
	@Enumerated(EnumType.STRING)
	private MemberType memberType;
	
	@ManyToOne
	@JoinColumn(name = "TEAM_ID")
	private Team team;
}
```

관계를 설정하는 네이밍 규칙은 `(현재 객체) To (타겟 객체)` 를 따른다. 그렇기 때문에 관계가 설정되는 Member 는 현재 객체가 되고, 이와 관계를 가지는 타겟 객체는 Team 이 된다. 

위 설명을 이해했다면 먼저 `@ManyToOne` 을 이용해 다대일 관계를 설정한다. Team 객체는 여러 개의 Member 를 가질 수 있다는 뜻이다. 이렇게 설정만 수행하면 아래와 같이 코드가 간단해진다.

```java
public class Main {
	public static void main(String[] args) {
		EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

		EntityManager em = emf.createEntityManager();
		EntityTransaction tx = em.getTransaction();
		tx.begin();

		try {
			Team team = new Team();
			team.setName("TeamA");
			em.persist(team);
			
			Member member = new Member();
			member.setName("member1");
			member.setTeam(team);
			em.persist(member);
			
			// 객체 참조를 통한 Team 조회
			Member findMember = em.find(Member.class, member.getId());
			Team findTeam = findMember.getTeam();
			
			tx.commit();
		}
        }
}
```

<br/>
<span style="color: gray; font-weight: 1.2em">FetchType.LAZY & FetchType.EAGER</span>

`em.flush(); em.clear();` 로 DB에 쿼리를 전부 보내도록 설정하면 `@ManyToOne(fetch = FetchType.LAZY)` 를 지정했을 때, 어떤 쿼리가 수행되는지 확인 할 수 있다.

`LAZY` 한 패치는 불러온 객체를 실제 사용할 때, 쿼리를 수행하도록 명령하는 것이다. 보통 모든 코드에 `LAZY` 를 발라놓고, 꼭 필요한 곳에서만 한 번에 가져와 성능을 확인해보며 최적화 하는 방법을 추천한다고 한다.

## 양방향 매핑

다시 Team 과 Member 와의 관계를 살펴보자. Member 가 어떤 팀에 속해있는지는 알 수 있다. 그런데 Team 에 어떤 **Member들**이 속해있는지는 아직까진 확인 할 수 없었다. 이제 Team 에서 Member 객체들을 참조하기 위해 Team 객체에 `@OneToMany` 를 추가한다.

```java
@Entity
@Getter
@Setter
public class Team {
	@Id @GeneratedValue
	private Long id;
	private String name;
	
	@OneToMany(mappedBy = "team")
	private List<Member> members = new ArrayList<Member>();
}
```

여기서 Main.java 코드를 다음과 같이 작성해보자. 이 때, `em.flush(); em.clear();` 를 지우고 한 번 실행해보자. 반영이 안 된다.

```java
public class Main {
	public static void main(String[] args) {
		EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

		EntityManager em = emf.createEntityManager();
		EntityTransaction tx = em.getTransaction();
		tx.begin();

		try {
			Team team = new Team();
			team.setName("TeamA");
			em.persist(team);
			
			Member member = new Member();
			member.setName("member1");
			member.setTeam(team);
			em.persist(member);
			
            /* *************** */
			em.flush();
			em.clear();
            /* *************** */
			
			// Team 조회
			Member findMember = em.find(Member.class, member.getId());
			Team findTeam = findMember.getTeam();
			
			for (Member memberCls : findTeam.getMembers() ) {
				System.out.println(memberCls.getName());
			}
			
			tx.commit();
		}

        /* ... */

		emf.close();
	}
}
```

`em.flush()` 는 반영하는 동작을 수행한다. `persistence context` 에 어떤 동작을 수행해야 하는지가 남아있는 상태에서 `em.flush()` 명령으로 실제 데이터베이스로 명령을 수행한다.

`em.clear()` 는 현재 `persistence context` 에 있는 Member 정보와 Team 정보를 지운다. `em.flush()` 만 수행한 상태로 조회를 해보면 역시 Team 에는 어떠한 Member 정보도 있지 않다. 데이터베이스에는 적용되었으나 다시 한 번 꺼내오는 작업이 수행되지 않은 것.

이것 때문에 한참 헤맸지만 그만큼 나중에 절대 안 까먹을 것 같은 느낌. 사고치고 3일 지난 뒤에 발견하는 것보다 이렇게 실수로 배울 수 있어서 다행.

### 연관관계의 주인과 mappedBy

객체와 테이블이 연관관계를 가지는 방법 차이를 이해하면 mappedBy 를 이해 할 수 있다. 객체 연관관계에는 멤버에서 팀으로, 팀에서 멤버로 두 가지 단방향 관계가 필요하다. 즉, 객체의 양방향 관계는 사실 양방향 관계가 아니라 서로 다른 단방향 관계 2개라는 뜻이다. 

반면 테이블은 FK(외래키) 하나로 두 테이블 멤버와 팀 모두의 연관관계를 관리 할 수 있다. 그러나 객체 관계에서는 문제가 생긴다. Member 를 바꾸면 MEMBER 테이블의 값이 바뀌는 것은 당연하다. 그런데 Team 객체 내부의 member 를 바꾸어도 이에 해당하는 MEMBER 테이블이 바뀌어야 할까? 에 대한 의문이 생긴다.

이를 해결하기 위해 연관관계의 주인(Owner)를 가지도록 하는 것이다. 굉장히, 아주 굉장히 중요한 개념이다. 양방향 매핑 규칙에서 객체의 두 관계 중 하나를 연관관계의 주인으로 설정하고 다른 하나는 조회만 가능하도록 만드는 것이다.

연관관계의 주인만이 외래 키를 관리하며 등록, 수정 동작을 수행 할 수 있다. 반면 주인이 아닌 쪽은 읽기만 가능하다. 주인은 mappedBy 속성을 사용하지 않고, 주인이 아닌 경우만 mappedBy 속성으로 주인을 지정해준다. 즉, 아래 코드에서는 `mappedBy` 가 적히지 않은 Member 객체가 Ownership 을 가진다.

너무 중요한 이유... 현업에서 업데이트 넣었는데 안 되는 이유 대부분이 여기서 발생한다고 한다. Mendix 에서도 Entity 간 Ownership 을 지정 할 수 있는데, 아마 그 개념이 이 개념인 것 같다. JPA 를 배우면서 특히 Mendix 를 다시 이해하는 계기가 되는 중...

그럼 누구를 주인으로 해야 하는가? 강의에서는 **외래 키가 있는 곳을 주인으로 정하는 것**을 권장한다. 즉, Member.team 을 연관관계의 주인으로 하는 것이다. Team.members 는 가짜 매핑으로 조회만 가능하도록 만드는 것이다.

만약 위처럼 Member.team 을 연관관계 주인으로 설정하고 Team.members 에만 add 를 해주게 되면 데이터베이스 상에서 TEAM_ID 값은 NULL 이 나타난다. 반대로 Member.team 에 Team 을 세팅만 해주면 Team.members 에 자동으로 추가가 된다.

그러나.. 현업에서는? 무조건 두 군데에 집어 넣는다. 객체 지향적으로 따져보면 모두 수행하는 것이 맞다. 그리고 나중에 꼭 **영속 상태**를 고려하면서 코드를 짜는 것이 좋다. 객체 재활용이 습관이 된 사람들은 매우...

## 연관관계 매핑 어노테이션

- `@ManyToOne`
- `@OneToMany`
- `@OneToOne`
- `@ManyToMany` : 제약 조건이 많아 `@ManyToOne` 과 `@OneToMany` 로 풀어서 사용하는게 일반적이다.
- `@JoinColumn`, `@JoinTable`

또한 상속 관계 매핑 어노테이션이 존재한다. 자세한 내용은 모 개발자의 JPA 표준 책에 잘 나와있다는 얘기를 계속 얘기 해주기도하고, 회사에서 지원해주는 포인트도 남아있고해서 바로 그냥 사버렸다.

아래 어노테이션을 사용하면 데이터 상속 관계도 표현 할 수 있으며, 데이터베이스의 슈퍼 타입, 서브 타입까지 표현 할 수 있다.

- `@Inheritance`
- `@DiscriminatorColumn`
- `@DiscriminatorValue`
- `@MappedSuperclass`

복합키 어노테이션은 아래와 같다. 키를 2개 이상 잡아서 복합키로 만드는 방법이다. 나중에 책보고 정리할 예정.

- `@IdClass`
- `@EmbeddedId`
- `@Embeddable`
- `@MapsId`