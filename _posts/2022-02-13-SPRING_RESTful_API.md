---
title: "[Spring] RESTful API"
excerpt: "RESTful API"
date: 2022-02-13
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - layered architecture
    - component
    - RESTful API
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# RESTful API

RESTful API 를 설명하기 전에 Open API(Application Programming Interface)를 알아보자. Open API는 말 그대로 개방형 API이다. API가 응용 프로그램을 개발할 때 사용하는 인터페이스라는 의미이므로, Open API는 프로그래밍에서 사용할 수 있는 개방되어 있는 상태의 인터페이스를 말한다. 이러한 Open API와 함께 자주 거론되는 기술이 REST이며, 대부분 Open API는 REST 방식으로 지원되고 있다.

그럼 이제 REST(REpresentational Safe Transferful) API를 알아보자. RESTful 하다는 것은 클라이언트의 모든 요청을 **HTTP URI + HTTP Method** 두 가지로 나타내는 것을 뜻한다. 즉, HTTP URI를 통해 제어할 자원을 명시하고, HTTP Method (GET, POST, PUT, DELETE)를 통해 해당 자원을 제어하는 명령을 내리는 방식의 아키텍처라고 할 수 있다. HTTP 프로토콜에 정의된 4개의 메서드들이 자원에 대한 CRUD Operation 을 정의한다.

|HTTP Mehod|CRUD|
|---|---|
|POST|Create(Insert)|
|GET|Read(Select)|
|PUT|Update(Modify)|
|DELETE|Delete|

RESTful API는 HTTP와 URI 기반으로 자원에 접근할 수 있도록 제공하는 어플리케이션 개발 인터페이스이다. 예를 들어 기존 게시판은 GET과 POST만으로 자원에 대한 CRUD 처리를 수행한다. `POST /bbs/regist` 와 같은 요청을 보내 글을 등록하게 되는데, 반면 RESTful API를 지원하는 게시판은 `POST /bbs` 로만 전달하면 요청한 동작이 수행된다. 즉, 어떠한 요청에 요청을 정의하는 동사가 포함되어 있지 않다는 것이 특징이다.

이런 형태 구조를 사용하면 JSON/XML 데이터 타입을 사용한다. 그 중 특히 JSON은 XML과 비교해 마크업에 따른 오버헤드가 적어 점차 많이 활용되는 추세다.

## JSON

JSON(JavaScript Object Notation) 에 대해 간략히 특징을 정리했다.

- http://www.json.org
- JSON은 경량(light-weight) DATA 교환 방식
- Javascript 에서 객체를 만들 때 사용하는 표현식 의미
- JSON 표현식은 사람과 기계 모두 이해하기 쉬우며 용량이 작아 XML 을 대체한 데이터 전송에 많이 사용
- 특정 언어에 종속되지 않으며, 대부분 프로그래밍 언어에서 JSON 포맷 데이터를 핸들링 할 수 있는 라이브러리 제공
- Name-Value 형식을 가짐
- 값들의 순서화된 리스트 형식을 가져 Hirachey 를 가진 모든 데이터 형식을 표현할 수 있음
- Jackson 라이브러리를 통해 JSON ↔ Java 객체 변환 가능

## Spring MVC 기반 RESTful 웹 서비스 구현 절차

구현 절차는 대체적으로 아래오 같은 순서를 가진다.

- RESTful 웹 서비스를 처리할 RestfulController 클래스 작성 및 Spring Bean 등록
- 요청을 처리할 메서드에 `@RequestMapping`, `@RequestBody`, `@ResponseBody` 어노테이션 선언
- REST Client Tool (Postman) 사용해 각각의 메서드 테스트
- Ajax 통신 사용해 RESTful 웹 서비스 호출하는 HTML 페이지 작성

이를 기업의 신규 SI가 진행되는 과정에 빗대어 보면.. UI/UX 현업 검수 이후 화면설계, 이후 백엔드 설계로 진행되는 기존 과정과 완전히 반대되는 것 같다. 정확한 요구사항 명세로부터 주고 받을 데이터를 정확하게 정의한 뒤 인터페이스를 구성한다. 즉, 백엔드 작업이 우선된다. 이 부분이 조금 어색한데.. 아마도 프로젝트를 많이 안 해봐서 그렇다고 느끼는 중이다.

RESTful Controlleer 구현을 위한 핵심 어노테이션 두 가지를 아래와 같이 정리한다. 클라이언트로부터 요청을 받을 때, 요청에 대한 응답을 수행할 때 사용한다.

- `@RequestBody` : HTTP Request Body (JSON) → Java 객체
- `@ResponseBody` : Java 객체 → HTTP Response Body (JSON)

## RESTful service 개발

`@GetMapping` 어노테이션으로 감싼 메소드가 객체를 반환하게 되면 Java 객체 정보 자체가 반환되어 오류가 발생한다. 그렇기 때문에 `@ResponseBody` 어노테이션을 사용해 Jackson 라이브러리가 반환되는 Java 객체를 JSON 형태로 자동 변환하도록 명시해야 한다. 이 때 사용되는 Jackson 라이브러리는 Maven central repository 에서 추가할 수 있다.

먼저 RESTful Controller 에서 요청을 처리할 부분을 아래와 같이 작성한다.

```java
@Controller
@RequestMapping("/bbs")
public class BbsControlloer {
	
	@Autowired
	private BbsService bbsService;
	
	@GetMapping("/{articleId}")
	@ResponseBody
	public Article viewDetail(@PathVariable String articleId) {
		Article article = bbsService.viewArticleDetail(articleId);
		return article;
	}
}
```

서비스 객체에 `viewArticleDetail()` 메소드와 DAO 메소드를 추가한 뒤 포스트맨으로 어떤 값이 반환되는지 확인해보자. 이 때 반환되는 값은 어떤 `articleId` 값이 요청되든 같은 형태로 반환되게 소스를 구현했다.

```json
/* 반환 형식 */
{
    "articleId": 10,
    "author": "lee",
    "title": "test",
    "content": "테스트 입니다"
}
```

반환형을 객체로 지정하면 자동으로 JSON 형태로 변환되어 클라이언트에 반환되는 것을 확인할 수 있다. 주소를 복사해 웹 브라우저 자체에서 붙여 넣어도 동일한 응답이 화면에 보여진다.

한 단계 더 나아가 글쓰기 요청을 처리해보는 에제를 구현해보려고 한다. JSON 형태로 들어온 요청을 그대로 응답으로 보내는 메소드다. 여기서 주의깊게 볼 점은 `@RequestBody` 부분이다. 해당 어노테이션을 통해 JSON으로 전달된 요청을 객체로 전환하는 것을 볼 수 있다. 이전에 Parameter 들을 전부 하나로 모아줬던 Command 객체 사용방법에서 앞에 `@RequestBody` 어노테이션을 붙여주면 된다.

```java
@Controller
@RequestMapping("/bbs")
public class BbsControlloer {
	
	@Autowired
	private BbsService bbsService;
	
	@PostMapping("/write")
	@ResponseBody
	public Article write(@RequestBody Article article) {
		Article a = article;
		return a;
	}
}
```

여기까지 RESTful API 정의와 구현 방법을 정리했다. 이제 대강 웹어플리케이션 백엔드 개발이 어떤 작업을 수행하게 될지 아주 조금 가닥이 잡히는 수준이다.