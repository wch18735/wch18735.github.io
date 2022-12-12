---
title: "[Todo App] REST API 인증"
excerpt: "jwt: json web token"
date: 2022-12-11
layout: single
classes: wide
category:
    - todo app
tag:
    - java
    - rest api
    - jwt
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# REST API 인증 기법

기본적인 백엔드 기능 개발을 수행한 다음은 인증(Certification)과 인가(Authorization) 영역을 다루게 된다. 내가 개발한 API를 모든 사용자가 활용하는 것을 방직하는 것이다.

## BASIC 인증

기본적인 인증 방법은 HTTP 요청 헤더의 Authorization: 부분에 아이디와 비밀번호를 콜론으로 이어 붙인 후 Base64로 인코딩한 문자열을 함께 보내는 것이다. 이 HTTP 요청을 수신한 서버는 문자열을 디코딩해 아이디와 비밀번호를 찾아낸 뒤, 데이터베이스 정보와 대조해 가입된 사용자인지 확인해본다. 구현은 쉽겠지만 조금만 생각해보면 금방 아래와 같은 문제들이 생길 수 있음을 떠올릴 수 있다.

1. 아이디/비밀번호 노출
2. 로그인 상태 유지 불가
3. 인증 과부하 가능성
4. 권한별 계정 테이블 운영
5. ...

가령 아래와 같은 시퀀스 다이어그램에서 클라이언트가 10만 개로 늘어난다고 가정해보자. 클라이언트는 비단 Device를 사용하는 사람일 뿐만 아니라 공장에서 돌아가는 기계 그 자체일 수 있다. 그렇기 때문에 10만 개의 접속 시도는 어떠한 형태로든 발생할 수 있는 일이다. 이에 따른 login GET 요청은 단숨에 DB를 먹통으로 만들고 만다.

<img src="/_img/2022-12-11/basic_auth.png">

## 토큰 기반 인증

토큰(Token)은 이해할 수 있는 단순한 문자열부터 다양한 문자들이 나열된 이해할 수 없는 문자열까지 다양한 형태로, 다양한 용도로 활용된다. 이 중 우리는 육안으로 읽어서는 절대 이해할 수 없으며, 간단한 디코딩 로직으로는 풀리지 않는 토큰을 사용해 사용자를 인증할 수 있다. 앞서 보았던 기본적인 인코딩 알고리즘을 활용한 것이 아니라 사용자와 클라이언트만 알 수 있는 암호화 로직을 사용한다고 가정해보자.

<img src="/_img/2022-12-11/token_auth.png">

이 방법은 아이디와 비밀번호를 노출하지 않으며, 토큰을 생성하고 인증하는 서버의 로직에 따라 유효기간이나 권한을 지정해 줄 수 있다. 그러나 한 가지, 인증 과부하 문제는 여전히 문제로 남아있다. 토큰 기반 인증 방식 역시 기존 방식과 마찬가지로 매 요청마다 DB Access를 수행하게 된다.

## JWT: JSON Web Token

전자 서명이란 특정 자료에 **서버가 가진 유일하며 해독 불가한 방법으로 암호화한 신호 또는 자료를 추가**하는 것이다. JWT은 전자 서명(Digital Signature) 방식 중 하나로 JSON 방식으로 암호화된 웹 토큰을 주고 받기 때문에 이와 같이 이름 붙여졌다. 이러한 JWT 토큰은 **{header}.{payload}.{signature}**로 구성되어 있다. 아래 자세히 보면 두 개의 점으로 구분된 각 부분을 확인할 수 있다.

```log
eyJhbGciOiJIUzI1NpXVCJ9.eyJpZCI6MTIzNDU2Nzg5LCJuYWzZXBoIn0.OpOSSw7e485LOP5PrzScxHb7SR6skfFwi4rp7o
```

이러한 JWT를 Decoding하면 아래와 같은 형태로 나타난다. 아래는 난수이며 위 코드를 해석한 것이 아니란 것을 알아두자.

```json
{   // header
    "typ": "JWT",
    "alg": "HS512"
}.
{   // payload
    "sub": "4513ab13813d21z53867sa101",
    "iss": "demo app",
    "iat": 1595733654,
    "exp": 1595733654
}.  // signature
Nndalkeionlsdvlkasnlnbijh
```

각 파트의 필드는 아래와 같으며 발행 과정은 생각보다 간단하다. 토큰은 기본적으로 서버가 생성해 클라이언트에게 전달한다. 만약 클라이언트가 로그인에 성공하면 서버는 사용자 정보를 **시크릿 키로 암호화**한 결과를 생성해 signature에 삽입하고, 해당 결과를 다시 클라이언트에 전달한다. 추후 클라이언트는 해당 토큰을 요청과 함께 전송하는데, 서버는 이전에 사용했던 시크릿 키로 암호를 해제해 요청과 함께 전달된 header, payload에서 사용자 정보를 뽑아 해독한 정보와 대조한다. 만약 두 값이 같다면 올바른 요청이라는 것이다.

- Header
    - typ: Type을 줄인 말로 이 토큰의 타입을 의미
    - alg: Algorithm을 줄인 말로 토큰의 서명을 발행하기 위해 사용된 해시 알고리듬 종류를 의미
- Payload
    - sub: Subject를 줄인 말로 이 토큰의 주인을 의미. sub은 ID처럼 유일한 식별자여야 한다.
    - iss: Issuer를 줄인 말로 이 토큰을 발행한 주체를 의미한다. 보통의 application 이름이 들어간다.
    - iat: Issued at을 줄인 말로 토큰이 발행된 날짜와 시간을 의미한다.
    - exp: Expiration을 줄인 말로 토큰이 말료되는 시간을 의미한다.
- Signature
    - signature: 토큰을 발행한 주체이며 Issuer가 발행한 서명의 실제 내용이다. 토큰의 유효성 검사에 사용된다.

## JWT 적용 구현

JWT를 통한 인증은 매 API가 호출될 때마다 가장 먼저 동작해야 한다는 것을 기억하며 구현 과정을 살펴보자. JWT 구현을 위해서는 먼저 JWT관련 라이브러리를 추가한다. build.gradle dependencies 부분에 Jjwt 라이브러리를 추가해준다.

```gradle
implementation group: 'io.jsonwebtoken', name: 'jjwt', version: '0.9.1'
```

라이브러리를 추가했으면 구현을 위해 가령 com.example.demo.security 패키지를 만들고, 그 안에 Token을 검증하고 제공해주는 TokenProvider 클래스를 만들어준다. 먼저 create 메소드를 보면 Header, Payload 정보를 설정하고 compact()를 통해 토큰을 발행하는 것을 확인할 수 있다. 반대로 validateAndGetUserId() 메소드에서는 전달받은 토큰을 SECRET_KEY를 이용해 해독하고, 저장했던 사용자 ID 부분을 가져오는 것을 확인할 수 있다.

```java
@Slf4j
@Service
public class TokenProvider {
	private static final String SECRET_KEY = "secretkey";
	
	public String create(UserEntity userEntity) {
		Date expiryDate = Date.from(Instant.now().plus(1, ChronoUnit.DAYS));
		return Jwts.builder()
				   .signWith(SignatureAlgorithm.HS512, SECRET_KEY)
				   .setSubject(userEntity.getId())
				   .setIssuer("demo app")
				   .setIssuedAt(new Date())
				   .setExpiration(expiryDate)
				   .compact();
	}
	
	public String validateAndGetUserId(String token) {
		Claims claims = Jwts.parser()
							.setSigningKey(SECRET_KEY)
							.parseClaimsJws(token)
							.getBody();
		
		return claims.getSubject();
	}
}
```

이렇게 만든 JWT는 API가 실행될 때마다 사용자를 인증해주어야 한다. 이를 위해 우리는 Spring Security를 사용한다. Spring Security는 서블릿 필터들의 집합으로, 스프링 부트가 실행하는 DispatcherServlet이 실행되기 전 실행되어 원하지 않는 HTTP 요청을 걸러내는 역할을 한다. 이와 관련된 [아주 정확한 설명](https://medium.com/geekculture/spring-security-in-motion-part-1-8fc2644244f)은 이곳에서 읽을 수 있다. 추후 여유있는 주말에 곧 번역 예정이다.

결론적으로 Filter를 이용해 API가 실행될 때마다 검사하는 기능은 `implementation 'org.springframework.boot:spring-boot-starter-security`을 추가한 뒤, 아래와 같이 구현할 수 있다. 이 때, 생성한 사용자 인증 정보는 SecurityContextHolder에 컨텍스트로 등록한다. 이 SecurityContextHolder는 ThreadLocal에 저장되는데, 이를 저장하는 이유는 요청을 처리하는 과정에서 사용자가 인증됐는지 여부나 인증된 사용자가 누구인지 식별이 필요할 때 정보를 제공하기 위함이다.

```java
@Slf4j
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter{
	
	@Autowired
	private TokenProvider tokenProvider;
	
	@Override
	protected void doFilterInternal(HttpServletRequest request, 
									HttpServletResponse response,
									FilterChain filterChain) throws ServletException, IOException {
		try {
			String token = parseBearerToken(request);
			log.info("Filter is running...");
			if(token != null && !token.equalsIgnoreCase("null")) {
				String userId = tokenProvider.validateAndGetUserId(token);
				log.info("Authenticated user ID : " + userId);
				AbstractAuthenticationToken authentication = new UsernamePasswordAuthenticationToken(
																	userId,	// AuthenticationPrincipal 
																	null,
																	AuthorityUtils.NO_AUTHORITIES);
				authentication.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
				SecurityContext securityContext = SecurityContextHolder.createEmptyContext();
				securityContext.setAuthentication(authentication);
				SecurityContextHolder.setContext(securityContext);
				} 
			}
		catch (Exception ex) {
			logger.error("Could not set user authentication in security context", ex); 
			}
		filterChain.doFilter(request, response);
	}
	
	private String parseBearerToken(HttpServletRequest request) {
		String bearerToken = request.getHeader("Authorization");
		if(StringUtils.hasText(bearerToken) && bearerToken.startsWith("Bearer ")) {
			return bearerToken.substring(7);
		}
		return null;
	}
}
```