---
title: "[Java] LocalDate, LocalTime and LocalDateTime"
excerpt: "representing date and time in java"
date: 2023-05-16
layout: single
classes: wide
category:
    - java
tag:
    - java
    - abstract
    - interface
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Date and Time

최근 개발자 친구가 해준 뼈를 때리는 한 마디 덕분에 다시 심기일전하여 개발 공부를 하고있다. 실제 개발에 들어가면 평소에 공부하지 않았던 다양한 문제를 접하게 된다. 그 중 하나가 **시간**은 어떻게 관리하고 표현해야 하는가에 대한 의문이다. 

나는 Date, Time를 직접 다뤄보기 이전엔 얼핏 `기본적으로 제공되는 라이브러리가 있었던 것 같으니 나중에 알아서 되겠지` 라고 생각했다. 그냥 Date 타입을 박았다가 대학 동기이자, 무료 과외 선생이자, 잠재적 경쟁자인 개발자로부터 이런 카톡을 받았다.

<img src="/_img/2023-05-16/kakao-talk-with-him.png">

그래서 나중에 보려고 LocalDate, LocalTime, LocalDateTime 세 클래스를 짤막하게 정리해본다. LocalDate, LocalTime, LocalDateTime 구현 예제를 기억하는 것도 좋짐만 글 가장 하단에 참조로 표시한 공식 문서를 읽어보면 큰 도움이 될 것 같다.

## LocalDate

LocalDate 클래스는 ISO-8601 time-zone 정보가 없는 날짜를 뜻한다. LocalDate는 내부 상태가 변경되지 않는 *immutable* 특성을 가지며 보통 `년-월-일` 형태로 날짜를 보여준다. 물론 다른 형태로도 보여줄 수 있으며, 가장 중요한 것은 그 형태가 어떻게 바뀌든 LocalDate 클래스는 *value-based class* 이기 때문에 *identity-sensitive* 기능에는 취약할 수 있다는 것을 명심하는 것이다.

## LocalTime

LocalTime 클래스는 ISO-8601 time-zone 정보가 없는 시간을 뜻한다. LocalDate와 마찬가지로 내부 상태가 변하지 않으며, 날짜와 관계없이 시간 정보를 보여준다. 가령 매일 6시마다 특정 동작을 수행하는 배치를 만들고 싶다면, LocalTime을 이용하면 간단하게 구현할 수 있다.

## LocalDateTime

LocalDateTime 클래스 역시 *immutable* 한 특성을 가지며, 날짜와 시간을 나타낸다. `System.out.println(LocalDateTime.now());` 코드를 통해 현재 시간을 간단히 출력해보면, 가운데 **T** 라는 알파벳이 들어간 것을 확인할 수 있다.

처음 이를 보면 당황할 수 있는데, 이는 LocalDate, LocalTime 두 형식을 합쳐 표현하기 위한 구분자라고 생각하면 된다.

## Summary

자세한 기능들은 Oracle에서 제공하는 [LocalDate](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html), [LocalTime](https://docs.oracle.com/javase/8/docs/api/java/time/LocalTime.html), [LocalDateTime](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html) 공식 문서를 보자.