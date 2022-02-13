---
title: "[Junit] Boundary Condition check"
excerpt: "boundary condition check by short term correct"
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

# 경계 조건을 고려하는 6가지 약어: CORRECT

단위 테스트는 종종 경계 조건들에 관계된 결함들을 미연에 방지하는 데 도움이 된다. 경계 조건은 **사용자가 의도한 동작 시나리오(행복경로)** 끝에서 발생하여 문제를 일으킨다.

아래 약어들은 테스트를 만들 때 고려해야 할 경계 조건들을 생각하는데 도움을 준다.

## [C]:  Conformance (준수)

데이터 요소가 특정한 양식을 따르는지 확인해보기.

`wch18735@somedomain.com`

이메일은 주소 이름과 도메인으로 나눠질 수 있다. 이때, `@` 기호를 중심으로 파싱하여 주소 이름이 특정 규칙을 만족하는지를 확인하는 작업을 거쳐야 한다. 가령 null 값이나 빈 문자열, Database 상에서 오류를 발생시키는 값이 추가될 수 있다.

이 밖에도 전화번호, 계좌번호, 파일 이름 등 양식있는 문자열 데이터의 경계 조건을 구해보고 테스트에 적용해본다.

## [O]: Ordering (순서)

구현된 코드가 정확한 순서를 반환하는지 확인한다. 책의 예시에는 가장 높은 점수를 가진 데이터가 반환되길 기대했으나 그러지 않아 에러가 발생한다.

`assertThat(ranked.toArray(), equalTo(new Profile[]{ langrsoft, smeltInc}));`

## [R]: Range (범위)

- **기본형 중독 (primitive obsession)**: 기본형 데이터 사용에 과도하게 집착하는 개발 스타일. 알고리즘 솔버에게 뚜렷하게 나타남.
- 각도, 넓이 등이 일정 값 이하에서 벗어나지 않도록 범위를 정하는 테스트코드
  - `Bearing`, `Rectangle` 등의 예시
  - `constrainsSidesTo` 사용자 정의 햄크레스트 사용 (p.150)
  - `TypeSafeMatcher<ClassType>` 상속해서 구현 (return False 이면 테스트 실패)
```java
package scratch;

import org.hamcrest.*;

public class ConstrainsSidesTo extends TypeSafeMatcher<Rectangle> {
   private int length;

   public ConstrainsSidesTo(int length) {
      this.length = length;
   }

   @Override
   public void describeTo(Description description) {
	   // 설명 추가
      description.appendText("both sides must be <= " + length);
   }

    
   @Override
   protected boolean matchesSafely(Rectangle rect) {
	   // match 되는 조건 확인
      return 
        Math.abs(rect.origin().x - rect.opposite().x) <= length &&
        Math.abs(rect.origin().y - rect.opposite().y) <= length;
   }
   
   @Factory
   public static <T> Matcher<Rectangle> constrainsSidesTo(int length) {
	   // ???
      return new ConstrainsSidesTo(length);
   }
}
```
- 어떠한 수치를 나타내는 관념을 객체로 나타내 그 안에 경계 조건을 삽입해 문서 자체로 관리하는 방법을 지향
```java
package scratch;

import iloveyouboss.*;

public class Bearing {
   public static final int MAX = 359;
   private int value;

   public Bearing(int value) {
      if (value < 0 || value > MAX) throw new BearingOutOfRangeException();
      this.value = value;
   }

   public int value() { return value; }

   public int angleBetween(Bearing bearing) { return value - bearing.value; }
}
```

**Q.** 에러는 어디서 발생하는가?
**A:** `size += 1` 안 해줘서 생기는 것 같음. 아래처럼 고쳐주면 해결.
```java
public void put(int key, T value) {
      if (value == null) return;
      
      int index = binarySearch(key, keys, size);
      if (index != -1 && keys[index] == key) 
         values[index] = value;
      else
         insertAfter(key, value, index);
      
      size += 1;
   }
```

## [R]: Reference (참조)

- 어떤 행위 이전에 필수적으로 가정되어야 할 것이 있는지 테스트하기
- Reference 는 참조, 참고하다도 있으면서 기본으로 삼다도 있는 듯
- 절차적인 과정을 검증하는 예시로 Gear 와 자동차의 현재 상태에 따라 변속이 허용되는지를 보여줌
  - 코드 내부가 어떻게 동작하는지는 알 수 없이, 테스트 결과가 변속을 허용했는지로 체크
```java
@Test
   public void ignoresShiftToParkWhileInDrive() {
      transmission.shift(Gear.DRIVE);
      car.accelerateTo(30);

      transmission.shift(Gear.PARK);

      assertThat(transmission.getGear(), equalTo(Gear.DRIVE));
   }
   
   @Test
   public void allowsShiftToParkWhenNotMoving() {
      transmission.shift(Gear.DRIVE);
      car.accelerateTo(30);
      car.brakeToStop();
      
      transmission.shift(Gear.PARK);
      
      assertThat(transmission.getGear(), equalTo(Gear.PARK));
   }
```
 
## [E]: Existence (존재)

- 주어진 값이 존재하지 않는 경계조건을 확인하는 테스트를 작성하자
- null 반환, 기대하는 파일이 없을 때, 네트워크 응답이 없을 때 등의 상황을 가정해보기

## [C]: Cardinality (기수)

- 올바르게 세기
- 울타리 기둥 오류 (fencepost errors)
- 보통 index 계산 중에 범하는 오류로 out of index 발생하면 볼 수 있음
- 0, 1, n 법칙을 통해 기수에서 발생하는 오류를 항상 체크할 수 있도록 코드 작성

## [T]: Time (시간)

- 동시성 작업의 수행이 필요할 때, 이 작업들이 의도된 순서대로 처리되는지 확인하는 테스트 작성

## 참고

- `System.arraycopy(src, srcPos, dest, destPos, length)`
  - **src:** 전송원 배열
  - **srcPos:** 소스 배열의 개시 위치
  - **dest:** 전송처 배열
  - **destPos:** 전송체 데이터 내의 게시 위치
  - **length:** 카피되는 배열 요소의 수
  - C++ `#include<cstring>` 내부 `memset()` 과 비슷한 동작

- Transmission 내부 구현
```java
package transmission;

public class Transmission {

   private Gear gear;
   private Moveable moveable;

   public Transmission(Moveable moveable) {
      this.moveable = moveable;
   }

   public void shift(Gear gear) {
      // begs for a state-machine implementation
      if (moveable.currentSpeedInMph() > 0 && gear == Gear.PARK) return; 
      this.gear = gear;
   }

   public Gear getGear() {
      return gear;
   }
}
```