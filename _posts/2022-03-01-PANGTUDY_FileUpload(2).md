---
title: "[Pangtudy-web] File Upload Issue (2)"
excerpt: "file upload implementation"
date: 2022-03-01
category:
    - pangtudy-web
tag:
    - web development
    - spring boot
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# Trouble Shooting

<span style="font-size: 1.2em; font-weight: bold">Component</span>
- `@Component` → `@Service`, `@Repository`, `@Controller` 로 나눠서 만들 수 있음
- 그냥 Bean 을 만드려는 것이면 @Component 로 만들고 Component Scan 해주면 된다

<span style="font-size: 1.2em; font-weight: bold">@RequiredArgsConstructor</span>
- 컨트롤러 내부 서비스 객체에 `@Autowired` 달았더니 `the blank final field may not have been initialized` 에러가 남
- final 필드가 붙은 키워드에 대해 생성자를 만들어주는데 final을 선언해놓고 값을 선언해놓거나 생성자로 주입 받지 않으면 에러가 발생
- private final 필드를 만들었던 부분에서 에러

<span style="font-size: 1.2em; font-weight: bold">private static final vs private final</span>
StackOverflow 답변을 가져오자면
> private final static -> create this variable only once. private final -> create this variable for every object. First one saves memory

<span style="font-size: 1.2em; font-weight: bold">서버 동작 순서</span>

설정파일 관리가 제일 힘든 듯. 일단 tomcat 이 돌아갈 때, web.xml 을 들린다는 것만 기억하고, 거기서 servlet 관련 설정은 어디에 저장되어 있는지, 데이터 관련 설정은 어디에 있는지 등을 연결함.

결론: web.xml 은 설정의 설정 파일? 인 느낌이다.

