---
title: "[Junit] 모의 객체 (Mock Object) 활용"
excerpt: "mock object"
date: 2022-02-13
category:
    - junit
tag:
    - tdd
    - unit testing
    - junit
    - tdd
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
classes: wide
comments: true
---

# 목 객체 사용

목 객체란?

> **모의 객체(Mock Object)** 란 주로 객체 지향 프로그래밍으로 개발한 프로그램을 테스트 할 경우 사용되는 **가짜** 객체. 

조금 자세한 설명은 아래와 같이 나와있다.

> 테스트를 수행할 모듈과 연결되는 외부의 다른 서비스나 모듈들을 실제 사용하는 모듈을 사용하지 않고 실제 모듈을 **흉내**내는 **가짜** 모듈을 작성하여 테스트 효율성을 높이는데 사용하는 객체.

이러한 모의 객체는 언제 사용될까? 주로 자동화된 테스트를 수행하기 어려운 곳에 사용된다고 한다. 아래는 그 예시들이다.

- **사용자 인터페이스(UI) 테스트:** 사용자 반응이 필요한 테스트를 수행할 경우 사용자가 직접 테스트에 참여해야 함. 이럴 경우 모의 객체를 이용해 **사용자 응답을 흉내내어** 사용자 개입 없이도 테스트를 수행할 수 있음.

- **데이터베이스 테스트:** 자료 변경을 수반하는 데이터베이스 작업은 테스트 수행 후 매번 데이터베이스 자료를 원래대로 돌려놔야 함. 조회 및 변경 작업이 주된 테스트 목표가 아닐 때, 위와 같은 수정 작업을 수반하지 않기 위해 데이터베이스 응답을 흉내내서 변경 없는 테스트 가능.

## 동작 환경보다 로직 테스트에 집중

테스트하려는 과제는 좌표를 기반으로 생성된 Address 객체가 반환되는지를 확인함.

AddressRetriever.java 의 retrive() 메서드 끝에 `return new Address(houseNumber, road, city, state, zip);` 가 붙어있는데 이를 테스트.

이때, **1) API 연결관련 Issue**, **2) 실제 호출에 따른 속도 이슈**를 피하고, **반환되는 Address 형식이 맞는가**에 대한 **로직**만을 테스트하기 위해 작은 기법 활용.

## 스텁(Stub) 활용

스텁(Stub)이란?

> 테스트 용도로 하드 코딩한 값을 반환하는 구현체

책의 테스트에서도 아래와 같은 형태로 나타남.

```java
// <Q>
// 여기서 () -> "String" 이 get() 메소드에 매칭되는 이유는
// Interface Http 안에 구현할 추상 메소드가 하나만 존재해서?

Http http = (String url) -> 
         "{\"address\":{"
         + "\"house_number\":\"324\","
         + "\"road\":\"North Tejon Street\","
         + "\"city\":\"Colorado Springs\","
         + "\"state\":\"Colorado\","
         + "\"postcode\":\"80903\","
         + "\"country_code\":\"us\"}"
         + "}";
```

GET 요청에 대한 응답으로 이 스텁을 반환한다. 이로써 위에서 실제 테스트 시 발생할 수 있는 2가지 문제(테스트 시간, API 유효성)를 해결한다.

즉, 테스트하려는 로직에 집중할 수 있게 되었다.

![image](https://user-images.githubusercontent.com/49026408/140643654-e58a890b-6a7f-40f8-a77c-e5941957b0fc.png)

이제 아래 테스트를 진행해서 **로직에 맞는 Address가 생성되는가**만을 확인할 수 있음.

※ 211p ~ 212p: 나도 책 쓸 수 있겠다, 라는 희망을 주는 부분. `책은 파파고가 써주는 거구나` 하고 생각했다.

## 인자 검증

Http 스텁은 get() 메서드에 전달되는 위도, 경도 값과 무관하게 동일한 문자열을 전달함. 

앞에서 규격에 맞지 않는 값이 입력으로 전달되는 경우가 존재하는데, 이것이 문제를 야기할 수 있음.

따라서 Http 클래스 get() 메서드에 전달되는 URL을 검증하는 보호절을 추가해 위와 같은 상황 예방한다.

```java
if (!url.contains("lat=38.000000&lon=-104.000000")) 
    fail("url " + url + " does not contain correct parms");
```

## Mock 도구를 사용한 테스트 단순화

앞선 stub을 Mock 객체로 변환하는 방법. 즉, 인자로 주어지는 값들의 유효성을 검사하는 것!

- **자료형 검사:** 테스트에 어떤 인자를 기대하는지 명시하기
- **유효성 검사:** get() 메서드에 넘겨진 인자가 기대하는 형식, 패턴, 길이 등을 만족하는 인자인지 검증

반환하는 Stub 값 앞 뒤로 이 검증과정을 매번 붙이는 것보다 한 번 정의해 관리하는 것이 효율적이다.

이를 **Mockito** 에 구현해 놓았다.

대표적인 예시로 `when(...).thenReturn(...)` 형식.

```java
when(http.get(contains("lat=38.000000&lon=-104.000000"))).thenReturn(
            "{\"address\":{"
            + "\"house_number\":\"324\","
           // ...
            + "\"road\":\"North Tejon Street\","
            + "\"city\":\"Colorado Springs\","
            + "\"state\":\"Colorado\","
            + "\"postcode\":\"80903\","
            + "\"country_code\":\"us\"}"
            + "}");
```

위 코드가 동작하는 방법이 조금 궁금해서 파봤다. 틀릴 수도 있어서 얘기를 나눠봐야 될 듯.

위 예시에서 `when()`을 타고 조금 들어가다보면 `MockingProgress`가 나오는데, 내 생각엔 에러가 나지 않고 Progress 들을 모두 통과하면 `thenReturn()`이 동작하고 아닌 경우 비정상 동작을 수행하는 것 같다.

## Mockito 의존성 주입

의존성 주입이란?

> 소프트웨어 엔지니어링에서 의존성 주입(dependency injection)은 **하나의 객체가 다른 객체의 의존성을 제공하는 테크닉**이다. "의존성"은 예를 들어 서비스로 사용할 수 있는 객체이다. 클라이언트가 어떤 서비스를 사용할 것인지 지정하는 대신, 클라이언트에게 무슨 서비스를 사용할 것인지를 말해주는 것이다.

이를 짧게 요약하면 다음과 같이 나타낼 수 있다.

> 의존성 주입의 의도는 객체의 생성과 사용의 관심을 분리하는 것이다. 이는 가독성과 코드 재사용을 높혀준다.

Mockito 의존성 주입 절차는 다음과 같다.

- `@Mock` 어노테이션 사용해 Mock 인스턴스 생성
- `@Injection` 어노테이션을 사용해 Mock 인스턴스가 주입될 대상 인스턴스 변수를 선언
- 대상 인스턴스를 인스턴스화 후 `MockitoAnnotations.initMocks(this)` 호출

책에서 제시되는 예시를 보면 알겠지만, 자동으로 주입할 목 객체와 주입 대상 인스턴스를 찾아줌.

## Mock 객체 사용시 유의사항

- Mock이 프로덕션 코드의 동작을 올바르게 묘사하는지
- 프로덕션 코드는 Mock 객체가 반환하는 것과 동일한 형태를 반환하는지 (예외 상황은 없는지)
- 프로덕션 코드가 Null을 반환하는 경우가 있는지
- 어떤 Mock 프레임워크를 사용하느냐는 핵심 문제가 아님
- 항상 프로덕션 코드와 Mock 테스트와의 Gap이 존재한다는 사실을 잊지 않아야 함

## Mockito 메서드 종류

- Mock() - 모의 객체를 생성하는 역할
- when() - 협력객체 메소드 반환 값을 지정해주는 역할(stub)
- verify() - SUT안의 협력객체 메소드가 호출 되었는지 확인
  - times() - 지정한 횟수 만큼 협력 객체 메소드가 호출 되었는지 확인
  - never() - 호출되지 않았는지 여부 검증
  - atLeastOnce() - 최소 한 번은 특정 메소드가 호출되었는지 확인
  - atLeast() - 최소 지정한 횟수 만큼 호출되었는지 확인
  - atMost() - 최대 지정한 횟수 만큼 호출되었는지 확인
  - timeOut() - 지정된 시간 안에 호출되었는지 확인

**※ verify 관한 짧은 예제 및 설명**

앞에서 보여주는 메소드들을 부연설명하는 예시 코드이다.

```
private List<String> mock = mock(List.class);

@Test
public void verifyMethod_basic() {
    String value = mock.get(0);
    String value2 = mock.get(1);

    verify(mock).get(0);
    verify(mock, times(2)).get(anyInt());
    verify(mock, atLeast(1)).get(anyInt());
    verify(mock, atLeastOnce()).get(anyInt());
    verify(mock, atMost(2)).get(anyInt());
    verify(mock, never()).get(2);
}
```

## UI Test with Espresso

![image](https://user-images.githubusercontent.com/49026408/140644813-e32685cf-509a-469b-a32c-e4aceb4d306b.png)

```java
import android.support.test.espresso.Espresso;
import android.support.test.rule.ActivityTestRule;
import android.support.test.runner.AndroidJUnit4;

import org.junit.Rule;
import org.junit.Test;
import org.junit.runner.RunWith;

import static android.support.test.espresso.action.ViewActions.click;
import static android.support.test.espresso.action.ViewActions.closeSoftKeyboard;
import static android.support.test.espresso.action.ViewActions.typeText;
import static android.support.test.espresso.assertion.ViewAssertions.matches;
import static android.support.test.espresso.matcher.ViewMatchers.withId;
import static android.support.test.espresso.matcher.ViewMatchers.withText;

@RunWith(AndroidJUnit4.class)
public class HelloWorldEspressoTest {

    @Rule
    public ActivityTestRule<MainActivity> mActivityRule = new ActivityTestRule(MainActivity.class);

    @Test
    public void listGoesOverTheFold() {

        //editText 에 Hello World! 입력하고 키보드를 내립니다.
        Espresso.onView(withId(R.id.editText)).perform(typeText("Hello World!"), closeSoftKeyboard());

        //textView 의 값이 "Hello World!" 인지 확인합니다.
        Espresso.onView(withId(R.id.textView)).check(matches(withText("Hello World!")));

        //button 을 클릭합니다.
        Espresso.onView(withId(R.id.button)).perform(click());
    }
}
```

위와 같은 형태로 Espresso를 이용해 UI 동작을 정의할 수 있음. 결과적으로 `Espresso + Mockito` 를 통해 **모듈 단위 테스트**를 진행할 수 있다.