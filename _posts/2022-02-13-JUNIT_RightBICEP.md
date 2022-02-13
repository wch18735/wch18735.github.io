---
title: "[Junit] Right - BICEP"
excerpt: "right bicep"
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

# Right - BICEP

무엇을 테스트 할 것인지에 대해 쉽게 선별하는 기준! 

- **Right:** 결과가 올바른가?
- **B:** 경계 조건(boundary conditions)에서 정확히 동작하는지 검사하기!
- **I:** 역 관계(inverse relationship)를 만족하는지 검사하기!
- **C:** 다른 수단을 활용하여 교차 검사 (cross-check)해보기!
- **E:** 오류 조건(error conditions)을 강제로 일어나게 할 수 있는지 확인해보기!
- **P:** 성능 조건(performance characteristics)은 기준에 부합하는지 확인하기!

## [Right] - BICEP: 결과가 올바른지 검사하자

테스트 코드는 무엇보다 **기대한 결과**를 산출하는지 검증할 수 있어야 함. 개발자가 **이렇게** 동작할 것이다, 라고 생각하는대로 코드가 동작해야 함.

따라서 가장 먼저 확인할 것은 **이 동작은 내가 예상되는 방식으로 동작하는가**이다.

## Right - [B]ICEP: 경계 조건을 검사하자

우리가 접하는 많은 에러들은 모서리 사례(corner case)들에서 발생함. 알고리즘 문제 풀이 고수들이 보통 이런 corner case example 들을 찾고, 제안하여 문제에 추가한다. 예시들은 다음과 같은 곳에서 발생할 수 있다.

- 모호하고 일관성 없는 입력 값. 특수 문자가 포함된 파일 이름 등.
- 잘못된 양식의 데이터. 헤더가 손상되어 잘 못 된 확장자를 가지는 경우.
- 수치적 오버플로우를 일으키는 연산
- 입력을 예상하는 곳에서 발생하는 NULL
- 이상적인 기댓값을 훨씬 벗어나는 값. 150세의 나이, 2m를 넘는 키 등
- 중복을 허용하지 않는 경우에서 발생하는 중복
- 정렬 알고리즘의 worst case 에서 발생하는 정렬 Delay
- 시간 순이 맞지 않는 경우

### 경계 조건에서는 CORRECT를 확인하기

- Conformance(준수): 값이 기대한 양식을 준수하는지
- Ordering(순서): 값의 집합이 적절하게 정렬되거나 정렬되지 않았는지 확인
- Range(범위): 이상적인 최솟값과 최댓값 사이에 있는지
- Reference(참조): 코드 자체에서 통제할 수 없는 외부 참조를 포함하는지 (ex: 해외 웹 서버에서 크롤링)
- Existence(존재): 값이 존재하는지, 추후에 존재하지 않을 가능성이 있는지
- Cardinality(기수): 정확히 충분한 값들이 있어 계속해서 중복된 결과를 내지 않는지
- Time(절대적 혹은 상대적 시간): 모든 것이 의도한대로, 순서대로 발생하는지

## Right - B[I]CEP: 역 관계를 이용해 검사해보기

수학 계산에서 많이 사용될 수 있음. 예를들어, 뉴턴 알고리즘을 이용해 제곱근을 구하는 경우를 생각해보자. 이 때 구한 값을 두 번 곱한 값이 이전의 값의 허용 오차 내부에 들어갈 수 있는지 확인. 아래 코드의 **approx** 가 **root n** 이 될 때까지 while 루프를 돈다고 해석할 수 있음.

```java
/***
 * Excerpted from "Pragmatic Unit Testing in Java with JUnit",
 * published by The Pragmatic Bookshelf.
 * Copyrights apply to this code. It may not be used to create training material, 
 * courses, books, articles, and the like. Contact us if you are in doubt.
 * We make no guarantees that this code is fit for any purpose. 
 * Visit http://www.pragmaticprogrammer.com/titles/utj2 for more book information.
***/
package scratch;

import org.junit.*;
import static org.junit.Assert.*;
import static org.hamcrest.number.IsCloseTo.*;
import static java.lang.Math.abs;

public class NewtonTest {
   static class Newton {
      private static final double TOLERANCE = 1E-16;

      public static double squareRoot(double n) {
         double approx = n;
         while (abs(approx - n / approx) > TOLERANCE * approx)
            approx = (n / approx + approx) / 2.0;
         return approx;
      }
   }

   @Test
   public void squareRoot() {
      double result = Newton.squareRoot(250.0);
      assertThat(result * result, closeTo(250.0, Newton.TOLERANCE));
   }
   
   @Test
   public void squareRootVerifiedUsingLibrary() {
      assertThat(Newton.squareRoot(1969.0), 
            closeTo(Math.sqrt(1969.0), Newton.TOLERANCE));
   }
}
```

## Right - BI[C]EP: 다른 수단을 활용하여 교차 검사할 수 있는지 확인하기

내가 예상하는 메서드가 **동일한 자료에 대해** 잘 만들어진 라이브러리, 프로그램에서 반환하는 결과와 같은 결과를 보여주는지 확인하기. 예를 들어, 내가 만든 `sqrt()` 메서드와 `Math.sqrt()` 가 비슷한 값을 반환하는지 확인해보는 방법!

책에서 설명하는 도서 대출 시스템 예제도 **도서관의 모든 책**에 대하여 대출된 도서를 체크하는 시스템과 선반에 있는 도서를 검사하는 시스템이 합쳐서 교차 검사할 수 있어야 함.

## Right - BIC[E]P: 오류 조건을 강제해 검사하는 방법

경계 조건과 비슷한데, 논리적이 corner case 를 상상하기 보다는 시스템 적인 한계 조건을 고려해보는 것.

- 메모리가 가득 차는 경우
- 디스크 공간이 가득 차는 경우
- 서버와 클라이언트 시간이 다른 경우
- 네트워크 가용성 및 오류
- 시스템을 로드하는 경우
- 색상 팔레트에 제한된 색상 밖에 존재하지 않는 경우
- 매우 높거나 낮은 비디오 해상도 디스플레이를 사용하는 경우

## Right - BICE[P]: 성능 조건이 기준에 부합하는지 확인해보기

실제로 병목 현상이 어디에 발생하는지는 코드의 다양한 부분에서, 예상하지 못 한 곳에서 발생할 수 있음. 따라서 **추측**만으로 성능 문제가 발생하는 지점을 예측하지 않아야 함.

대신! 실제 데이터를 사용해 **실제 경과 시간**을 확인해봐야 한다! 성능이 개선되는지 확인하고 싶다면? 코드 개선 전에 성능을 측정해보고, 동일한 방법으로 성능 개선이 이뤄지는지 확인해보기! JMeter, JUnitPerf 같은 도구를 찾아서 활용할 수 있음.

## Questions

- 129p `루틴 두 개가 공통된 코드를 호출한다면 프로덕션 코드와 역 행동 모두 같은 결함을 공유하게 됩니다.` 의 뜻?