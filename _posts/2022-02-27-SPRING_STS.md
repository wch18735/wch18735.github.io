---
title: "[Spring Boot] STS Problem (1)"
excerpt: "not automatically install lombok"
date: 2022-02-27
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

<span style="font-size:1.5em">"그거 STS 문제일 수 있음. IntelliJ 쓰셈."</span>

설마 했는데 진짜로 그 문제였다. 정말 끔찍한 일이 아닐 수 없다. 이틀동안 이것만 팠는데...

문제는 `@RequestBody` 확인 과정에서 시작됐다. JSON 으로 들어오는 요청의 Body 값과 DTO 값이 자동으로 매핑이 잘 되는지, LocalDateTime 은 형식이 잘 맞는지 확인해보려고 했는데... 이게 웬걸... 그대로 return 했는데도 LocalDateTime 밖에 반환이 안 되네...

그래서 LocalDateTime 을 지우고 반환하려고 해보니까 이번엔 `@ResponseBody could not find acceptable representation` 에러가 뜨네. 이건 `@Getter` 랑 `@Setter`가 없어서 뜨는 문제라고 했는데.. 이상했다. 나는 분명 DTO 위에 어노테이션으로 달아놨었기 때문이다.

lombok 문제인가 싶어 그대로 `sts lombok` 이라고 검색하니까 이미 비슷한 경우를 겪고 있는 사람들이 많았다. 많은 해결 방법이 있었는데, 내 경우엔 STS Maven Library 내부 lombok jar 파일을 Run as Java Application 으로 실행한 다음 STS 실행 폴더의 ini 파일을 select 해 update/install 해주었다.

생각해보니 자동완성에 get 이나 set 메소드가 안 떴었는데.. 앞으로 이런 이슈가 많을 것 같아서 `(1)`을 앞에 붙였다. 험난한 개발이 될 것 같다.