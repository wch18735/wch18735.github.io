---
title: "[Spring] Controller"
excerpt: "spring mvc controller"
date: 2022-02-12
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - dispatcher servlet
    - controller
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# DispatcherServlet

DispatcherServlet 은 프론트 컨트롤러 역할을 수행한다. 웹 어플리케이션 최전방에서 사용자 요청을 접수하고 공통 기능을 처리한다. 이후 매핑된 URL 기준으로 요청을 처리할 Controller 를 찾는다. 아래는 DispatcherServlet 이 동작하는 그림이다.

<br/>
<img src="/_img/2022-02-12/DispatcherServlet.png" style="margin:auto auto;"/>
<br/>

요청이 전달되는 Controller 을 설명해보자. 프론트 컨트롤러는 사용자 요청(URL 기반)에 해당하는 컨트롤러로 요청을 전달하고, 요청을 받은 컨트롤러에서는 특정 메소드가 호출된다. **개발자는 이 실행되는 메소드를 주로 관리하게 된다.** 

한 가지 주의할 점이 있다. 컨트롤러 내부에서 서비스를 구현하는 것이 아니라는 것. 비즈니스 처리를 위해서는 서비스 컴포넌트를 주입 받아 실행하게 된다. 그리고 이렇게 메서드가 수행된 후 발생한 결과를 컨트롤러에 반환한다. 결론적으로 `@Controller`, `@RequestMapping`, `@Autowired` 세 가지 어노테이션을 사용한다고 볼 수 있다.

## @RequestMapping

리퀘스트 매핑 예제는 아래와 같다. 요청은 클래스 레벨에서도 가능하고, 메소드 레벨에서도 가능하다. 또한 HTTP 요청은 URL과 더불어 요청 방식을 구분하는데, RequestMapping 어노테이션은 주로 자주 사용되는 GET, POST, PUT, DELETE 등을 구분할 수 있도록 지원한다. Spring 4.3 이후는 아예 `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping` 등을 사용할 수 있다.

```java
@Controller
@RequestMapping("/bbs")
public class BbsController{
    @RequestMapping("list")
    public String list(){
        return "bbs/list";
    }

    @RequestMapping(value="/write", method="RequestMethod.POST"){
        public Sring doWrite(){
            return "bbs/write_ok";
        }
    }
}
```

리퀘스트 매핑에서는 요청 URL을 파마리터로 사용하는 것도 가능하다. 먼저 경로변수를 사용하는 예시다. 요청 URL 형태가 `/bbs/{articleId}` 라고 할 때, 아래처럼 파라미터로 처리할 수 있게 된다. Vue.js Router 내에도 `/:board_id/:page` 등으로 파라미터를 요청 받았던 기억이 새록새록 난다. 똑같이 생각하면 된다.

```java
@Controller
@RequestMapping("/bbs")
public class BbsController{
    @Autowired
    private BbsService service;

    @RequestMapping("/{articleId}"){
        public String viewDetil(@PathVariable String articleId){
            Article article = service.viewArticle(articleId);

            return new ModelAndView("bbs/view_detail").addObject("article", article);
        }
    }
}
```

## ModelAndView

앞선 예제에서 ModelAndView 를 반환하는 모습을 살펴봤다. ModelAndView 는 응답으로 View(보통 JSP) 와 Model(Domain Object, VO)을 함께 전달할 때 사용한다. 서비스로부터 받아온 데이터를 뷰로 전달하는 역할을 수행하는 모습을 아래 예제에서 간단히 확인할 수 있다.

`setViewName` 에 적어준 경로가 곧 해당 jsp 를 view 로 지정하겠다는 것이다. 해당 jsp 에 전달할 데이터는 `addObject` 메소드를 통해 바인딩 해줄 수 있다. 전달받은 데이터는 jsp 파일 안에서 `${ modelName }` 을 통해 사용한다.

```java
@Controller
public class BbsController{
    @Autowired
    private BbsService service;

    @RequestMapping("/list")
    public ModelAndView list(){
        List<Article> articleList = service.getAllArticles();

        ModelAndView view = new ModelAndView();

        view.setViewName("bbs/list");
        view.addObject("articleList", list);

        return view;
    }
}
```

## Redirect & Forward

리다이렉트와 포워드는 웹 어플리케이션을 개발하면서 자주 사용하는 기능이다. Vue.js 에서는 `$router.push("path")` 이런 방식으로 썼던 것 같다. 동일한 기능이다. 컨트롤러에서 클라이언트 요청을 처리한 후 다른 페이지로 리다이렉트 (이쪽으로 가세요) 하고 싶을 때는 아래와 같은 방법으로 간단히 수행 가능하다.

- `return "redirect:/bbs/list";`
- `return "forward:/bbs/list";`
- `return "redirect:http://localhost:8080/TotalTest"`

이 때, 경로부분이 `/`로 시작하면 웹 어플리케이션 내에서 절대 경로이며, 그렇지 않은 경우 `@RequestMapping` 어노테이션 경로를 기준으로 상대 경로로 리다이렉트 한다. 끝으로 전체 경로를 적으면 해당 URL로 리다이렉트 된다.

## 간단한 Controller 구현

이번에는 Controller 작성, Postman HTTP Client 사용한 HTTP 파라미터 활용 두 가지를 다뤄본다. 참고로 Postman HTTP Client는 테스트 도구다. 테스트 하려고 버튼 만들고, 분석하는 작업이 필요없는 깔끔한 툴!

아래와 같이 컨트롤러를 작성한다. 그리고 서버를 재시작한 후 POSTMAN에서 서버로 요청을 날려보자.

```java
@Controller
@RequestMapping("/bbs")
public class BbsControlloer {
	
	@Autowired
	private BbsService bbsService;
	
	@RequestMapping("/write")
	public String write() {
		bbsService.registerArticle(new Article());
		
		return "write_ok";
	}
}
```

이렇게 원하는 값을 반환하는 것을 확인할 수 있다.

<img src="/_img/2022-02-12/postman.jpg">

다음은 파라미터를 전달해보려고 한다. POSTMAN 요청하는 옆 Param 을 눌러 전달하려는 값들을 나타내보자. GET 으로 보내려는 URL에 파라미터 값들이 추가되는 것을 확인할 수 있다.

위의 컨트롤러에 `@RequestParam` 을 사용해 파라미터를 통해 전달되는 값을 받아와 사용할 수 있다. 이 때, 설정한 `@RequestParam` 값이 전해지지 않으면 null 이 전달된다. 아래 컨트롤러를 실행시킨 뒤 요청을 보내보면 콘솔에 정상적으로 `author` 값이 전달되는 것을 확인할 수 있다.

```java
@Controller
@RequestMapping("/bbs")
public class BbsControlloer {
	
	@Autowired
	private BbsService bbsService;
	
	@RequestMapping("/write")
	public String write(@RequestParam("author") String author) {
		bbsService.registerArticle(new Article());
		System.out.println(author);
		return "write_ok";
	}
}
```

## @RequestParam

HTTP 요청에 전달되는 Parameter 들을 쉽게 처리할 수 있도록 도와준다. 아래 어노테이션을 활용한 예를 살펴보자. `require = false` 는 요청 값이 없을 때, 에러를 보내지 않고 null 을 리턴하며, `defaultValue=""` 를 통해 기본 값을 지정해 줄 수 있다.

```java
@RequestMapping("test8")
public ModelAndView test8(@RequestParam("id") String id, required=false, defaultValue=""){
    ModelAndView view = new ModelAndView();
    view.setViewName("test/test8");
    view.addObject("userId", id);
    return view;
}
```

이렇게 단순한 가정에서 파라미터 값이 20개가 넘어가는 실무에서의 상황을 고려해보자. 모든 값을 전부 메소드에 넣기에는 무리가 있다. 이렇게 많은 파라미터를 전송하는 경우 우리는 Command 객체를 사용한다. 파라미터의 이름과 동일한 프로퍼티를 가진 클래스를 작성해 사용한다.

예를 들어 email, name, password, confirmPassword 네 가지 파라미터를 form 전송 요청이 들어올 때, 이를 처리하기 위해 아래와 같은 Command 객체를 작성해 사용한다. 이렇게 만든 객체를 `@RequestMapping` 메소드의 인자에 삽입해 놓으면 자동으로 바인딩이 된다.

```java
public class Member {
    private String email;
    private String name;
    private String password;
    private String confirmPassword;

    public String getEmail(){
        return email;
    }

    // ... Getter / Setter
}
```

일반적으로 HTTP 요청은 String 형태로 전달되는데 Command 객체를 사용하면 정의해 놓은 데이터 타입으로 자동 전환되는 장점이 있다. 여기에 더해 같은 `name` 속성으로 전달된 여러 값들은 List 형태로 자동 바인딩이 된다.

## 간단한 HTTP 파라미터 처리 예제

위에서 배운 내용을 활용해 아래 네 가지를 간단하게 설명해보려고 한다.

- GET 방식, POST 방식 처리 방법
- path variable 처리
- command 객체 사용 방법
- ModelAndView 로 JSP 페이지에 데이터 전달 방법

먼저 GET 방식과 POST 방식을 처리하는 방법은 위에서도 다룬 바 있다. 기존 servlet 에서는 service 를 구현할 때, `doPost` 와 `doGet` 을 오버라이드하면 자동으로 요청 타입에 바인딩된 메소드가 실행됐다. 마찬가지로 지금은 어노테이션으로 구분하면 된다.

```java
@Controller
@RequestMapping("/bbs")
public class BbsControlloer {
	
	@Autowired
	private BbsService bbsService;
	
	@PostMapping("/write")
	public String doWrite(@RequestParam("author") String author) {
		bbsService.registerArticle(new Article());
		System.out.println("POST REQUEST");
		return "write_ok";
	}
	
	@GetMapping("/write")
	public String write() {
		bbsService.registerArticle(new Article());
		System.out.println("GET REQUEST");
		return "write_ok";
	}
}
```

파라미터는 Key and Value 쌍으로 전달되는 경우에 사용 가능하다. 그러나 사용자 요청은 단순히 URL 상에서 path variable 을 통해 전달되기도 하는데, URL 상에서 전달되는 값을 받아 사용하는 방법으로 `{}` 브라켓과 `@PathVariable` 을 사용하는 방법을 익혀보자.

```java
@Controller
@RequestMapping("/bbs")
public class BbsControlloer {
	
	@Autowired
	private BbsService bbsService;
	
	@GetMapping("{pageNum}/{articleId}")
	public String viewDetail(@PathVariable String pageNum, @PathVariable String articleId) {
		System.out.println("Page : " + pageNum);
		System.out.println("Article Id :" + articleId);
		
		return "write_ok";
	}
}
```

다음과 같이 컨트롤러를 작성한 뒤 `http://localhost:8080/hello-web/bbs/2/3` 을 GET 요청으로 보내면 콘솔 창에 의도했던대로 페이지 번호는 2번, 기사 번호는 3번이 전달되는 것을 확인할 수 있다.

다음은 요청을 진행했을 때, 클라이언트기 요청한 파라미터가 아주 많은 경우를 Command 객체를 통해 해결하는 예제다. 먼저 DTO로 사용할 Article 객체를 다음과 같이 정의한다.

```java
package com.example.hello.vo;

public class Article {
	private int articleId;
	private String author;
	private String title;
	private String content;
	
	public Article() {
		
	}

	public Article(int articleId, String author, String title, String content) {
		this.articleId = articleId;
		this.author = author;
		this.title = title;
		this.content = content;
	}

    /* .. Getter & Setter */

	@Override
	public String toString() {
		return "Article [articleId=" + articleId + ", authorString=" + author + ", titleString=" + title
				+ ", content=" + content + "]";
	}	
}
```

이렇게 만든 DTO 값을 아래 예시처럼 `@RequestMapping` 된 메소드 인자에 넘겨주기만 하면 POST 바디로 들어온 파라미터들과 이름이 같은 것들끼리 자동으로 바인딩 된다. 서비스 내부에 자동 주입받은 ArticleDAO 를 사용해 인자로 전달받은 article 오브젝트를 출력해보면 정상 동작함을 확인할 수 있다.

```java
@Controller
@RequestMapping("/bbs")
public class BbsControlloer {
	
	@Autowired
	private BbsService bbsService;
	
	@PostMapping("/write")
	public String doWrite(Article article) {
		bbsService.registerArticle(article);
		return "write_ok";
	}
}
```

이 때, 한글로 전달한 파일이 깨지는 경우가 있다. 당황하지말고 web.xml 내부 web-app 태그 안에 아래 필터를 삽입하자. 스프링 프레임워크에서 제공하는 기본 필터를 사용해 들어오는 요청 값들을 모두 사용자가 원하는 값으로 인코딩 한다. POSTMAN 에서는 `x-www-form-urlencoded` 를 사용한다.

```xml
<filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

마지막으로 jsp 페이지로 전달하는 방법이다. 지금까지는 String 문자열을 반환했으나 ModelAndView 를 반환하도록 고치고, 해당 ModelAndView 값에 데이터를 채워 넣는 과정을 추가해준다.

```java
@Controller
@RequestMapping("/bbs")
public class BbsControlloer {
	
	@Autowired
	private BbsService bbsService;
	
	@PostMapping("/write")
	public ModelAndView doWrite(Article article) {
		bbsService.registerArticle(article);
		System.out.println("POST REQUEST");
		return new ModelAndView("write_ok").addObject("article", article);
	}
}
```

이렇게 전달된 데이터를 출력하는 `write_ok.jsp` 파일을 아래와 같이 바꿔주면 포스트맨에서 바디에 담아 전달한 파라미터가 출력되는 것을 확인할 수 있다.

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>write ok..</h1>
	<ul>
		<li>${article.articleId}</li>
		<li>${article.author}</li>
		<li>${article.content}</li>
		<li>${article.title}</li>
	</ul>
</body>
</html>
```