---
title: "[Spring] Interceptors"
excerpt: "interceptors"
date: 2022-02-25
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - interceptors
    - transaction
author: 1FeS
comments: true
---

# Spring Interceptors

인터셉터는 모든 요청에서 반드시 거쳐야 하는 서블릿 필터와 유사하게 동작한다. 그러나 Servlet Filter 와 다른 점은 인터셉터는 컨트롤러 요청에만 적용된다는 것과 Bean Container 로딩 후 적용되기 때문에 빈을 사용할 수 있다는 것이다.

위 그림을 보면 사용자 요청에 따라 Filter, Dispatcher, Interceptor 가 어떤 순서로 배치되고, 적용 영역이 어디인지 확인할 수 있다.

<img src="/_img/2022-02-25/request_cycle.png">

따라서 설정 위치도 달라지는 것이 특징인데, Servlet Filter 는 web.xml 에 설정하는 반면 Interceptor 는 spring-servlet.xml 과 같이 servlet 을 담당하는 설정 파일에 작성하게 된다.

인터셉터의 공통 기능은 아래와 같이 작성할 수 있다. 아래 그림과 더불어 보면 어떤 단계에 기능이 수행되는지 대응시키기 조금 더 쉬울 것이다.

- preHandle : 컨트롤러(Handler) 실행 전
- postHandle : 컨트롤러(Handler) 실행 후, 뷰 실행 전
- afterCompletion : 뷰 실행 이후

<img src="/_img/2022-02-25/interceptor_sequence.png">

이런 인터셉터는 `로그인 처리` 또는 `웹 요청 처리시간 측정` 등의 기능 구현에 활용될 수 있다.

## 실행시간 측정 예제

먼저 Interceptor 를 아래와 같이 작성한다. 이 때, org.slf4j 로부터 Logger 와 LoggerFactory 를 받아와야 한다. 아래는 preHandle 과 afterCompletion 메소드를 정의하고 내부에서 `System.currentTimeMillis()` 를 이용해 시간을 계산한다.

```java
/* import ... */

public class MeasuringInterceptor extends HandlerInterceptorAdapter{
	
	Logger logger = LoggerFactory.getLogger(MeasuringInterceptor.class);
	
	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
		request.setAttribute("mi.beginTime", System.currentTimeMillis());
		return true;
	}

	@Override
	public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
			throws Exception {
		long beginTime = (long)request.getAttribute("mi.beginTime");
		long endTime = System.currentTimeMillis();
		
		logger.debug(request.getRequestURI() + "실행시간: " + (endTime - beginTime) + "ms");
	}
}
```

spring-context.xml 파일에 설정은 아래와 같이 기재해준다. 그리고 서버 실행 후 동작시키면 동작 시간이 측정되어 나타난다.

```xml
<!-- interceptor -->
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/bbs/**/"/>
        <bean id="measuringInterceptor" class="com.example.hello.interceptor.MeasuringInterceptor"></bean>
    </mvc:interceptor>
</mvc:interceptors>
```