---
title: "[Spring Boot] CORS 해결"
excerpt: "Fix CORS (Cross Origin Resource Sharing)"
date: 2022-06-07
layout: single
classes: wide
category:
    - spring boot
tag:
    - spring
    - interceptors
    - properties
    - cors
author: 1FeS
comments: true
header:
    teaser: "/_img/thumb-nail/spring-boot.png"
---

# CORS (Cross Orign Resource Sharing)

**CORS(Cross Origin Resource Sharing, 교차 출처 리소스 공유)** 는 프로젝트를 진행하며 꼭 한 번 만나게 된다. Postman 으로는 잘 되는데 왜 다른 클라이언트에서는 안 될까? 하면 제일 먼저 의심해보게 된다.

<img src="/_img/2022-06-07/cors_error.png"/>

CORS는 추가 HTTP 헤더를 사용해 한 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제라고 설명된다. 즉, 클라이언트가 다른 서버의 리소스를 얻기 위한 HTTP 요청을 실행할 때 발생한다. 

그러니까, 내가 `http://pangtudy.xyz:10833/post/3` 을 통해 게시글 정보를 요청하거나 첨부파일을 전송하는 경우 사용한다. 왜? 보안 상의 이유다. 

다른 출처의 리소스를 불러오려면 그 출처에서 올바른 CORS 헤더를 포함한 응답을 반환해줘야 비로소 요청이 가능한 것이다. 조르기 시전을 하려면 공손하게 `"제가 이걸 해달라고 땡깡 한 번 부려도 되겠습니까?"` 하고 물어보고, 허락을 받은 뒤 비로소 요청을 보낼 수 있다는 뜻.

이렇게 CORS 요청을 진행할 때, 가능한 메소드나 출처 등을 반환하는 헤더를 먼저 요청하는 것을 **Preflight Request** 라고 한다.

그럼 출처가 다르다는 뜻은 뭘까? 요청 URL과 응답 URL의 구성 요소 `Scheme`, `Host`, `Port` 3가지 중 하나라도 다르다면 다른 출처라 판단한다. Scheme 은 http, https 와 같은 프로토콜 스키마를 뜻한다. Host 는 URI 에서 제공하는 호스트, 가령 pangtudy.xyz 를 말한다. 마지막으로 Port 는 우리가 아는 그 포트가 맞다.

이번 프로젝트에서 이 CORS 가 발생해도 서버에는 정상적으로 로그가 남고, 파일도 전달되고, DB 업데이트도 일어나는 것을 확인할 수 있었다. 이를 잘 설명한 것이 아래 글이다. [이곳](https://evan-moon.github.io/2020/05/21/about-cors/)에 가면 명쾌한 설명과 경험으로 빚어낸 글을 읽을 수 있다.


<span style="font-weight: bold; font-size: 0.8em">여기서 중요한 사실 한 가지는 이렇게 출처를 비교하는 로직이 서버에 구현된 스펙이 아니라 브라우저에 구현되어 있는 스펙이라는 것이다.</span>

<span style="font-weight: bold; font-size: 0.8em">만약 우리가 CORS 정책을 위반하는 리소스 요청을 하더라도 해당 서버가 같은 출처에서 보낸 요청만 받겠다는 로직을 가지고 있는 경우가 아니라면 서버는 정상적으로 응답을 하고, 이후 브라우저가 이 응답을 분석해서 CORS 정책 위반이라고 판단되면 그 응답을 사용하지 않고 그냥 버리는 순서인 것이다.</span>

<span style="font-weight: bold; font-size: 0.8em">서버는 CORS를 위반하더라도 정상적으로 응답을 해주고, 응답의 파기 여부는 브라우저가 결정한다.</span>

<span style="font-weight: bold; font-size: 0.8em">즉, CORS는 브라우저의 구현 스펙에 포함되는 정책이기 때문에, 브라우저를 통하지 않고 서버 간 통신을 할 때는 이 정책이 적용되지 않는다. 또한 CORS 정책을 위반하는 리소스 요청 때문에 에러가 발생했다고 해도 서버 쪽 로그에는 정상적으로 응답을 했다는 로그만 남기 때문에, CORS가 돌아가는 방식을 정확히 모르면 에러 트레이싱에 난항을 겪을 수도 있다.</span>

<span style="font-weight: bold; font-size: 0.8em; color:gray">- Evans Library 中</span>

## 해결

몇 가지 해결방법이 있는데, 나는 WebConfig에 CORS 설정을 해주는 것으로 문제를 해결했다. 프로젝트 동일 레벨에 config 패키지를 만들고 WebConfig.java 파일을 추가해주자. 그리고 아래와 같이 설정만 해주면 끝.

```java
package com.example.demo.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.http.HttpMethod;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry corsRegistry) {
        corsRegistry.addMapping("/**")
                .allowedOrigins("http://localhost:8080")
                .allowedMethods(
                HttpMethod.GET.name(),
                HttpMethod.POST.name(),
                HttpMethod.OPTIONS.name(),
                HttpMethod.HEAD.name(),
                HttpMethod.PUT.name(),
                HttpMethod.DELETE.name()
                ).maxAge(3600);
    }
}
```

`HttpMethod.OPTIONS.name()` 을 주는 것과 allowedOrigins 를 직접 기입하는게 중요한 것 같았다. `*` 로 잡아줬는데 동작을 안 했다.