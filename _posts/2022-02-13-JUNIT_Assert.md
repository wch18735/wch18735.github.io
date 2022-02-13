---
title: "[Junit] Assert"
excerpt: "reason why we need to use unit testing"
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

## 배경 지식

- assert: ~이 있다고 확신하다
- JUnit에서 단언은 정적 메서드 호출 (데이터 영역에서 호출된다(?)는 뜻으로 해석해도 될까)
- 각 단언은 **어떤 조건이 참인지 검증**하는 방법 (참이 아니면 실패 보고함)
- **src** 와 **test** 폴더 나눠서 진행하는 구조

## JUnit 두 가지 단언 스타일

- matchers: 전통적인 방식
- hamcrest: 전통적 방식에 더해 더욱 다양한 비교 가능

## assertTrue

- True 가 있다고 단언하다 (False 이면 실패 반환)
- 기본적인 단언 형태는 `org.junit.Assert.assertTrue(someBooleanExpression)`
- 보통은 전부 import 해서 사용함 `import static org.junit.Assert.*` (iloveyouboss_3 예제 코드도 이렇게 사용)

## assertThat

- 명확한 값을 비교함
- `assertThat( account.getBalance(), equalTo(100) )`
  - 대표적인 햄크레스트(hamcrest) 단언 예시
  - 첫 번째 인자는 실제 표현식 (검증하고자 하는 값이나 메서드)
  - 두 번째 인자는 매처 (matcher; 실제 값과 표현식의 값을 비교함)
  - 매처는 코드를 왼쪽 → 오른쪽으로 읽게하여 일반 문장처럼 읽을 수 있게 해줌
  - 위는 `계좌 잔고가 100과 같음을 확신한다` 로 해석 가능
- JUnit이 제공하는 핵심 햄크레스트는 코드에 정적 임포트 추가해야함
  - `import static org.hamcrest.CoreMatchers.*;`

## Why hamcrest?

일반적인 단언보다 햄크레스트 단언이 테스트 실패 시 오류 메세지에서 더 많은 정보를 주기 때문에.

## is decorator

- `is( matcher )` 를 사용해서 좌 → 우로 읽을 때 더 편하게 이해할 수 있음

## not matcher

어떤 것을 부정하기 위해 사용함
- `is(not(matcher))` 과 같이 사용할 수 있음

## 부동소수점 비교

- `assertThat(2.32 * 3, equalTo(6.96)) ` 은 실패함
- `isCloseTo` 매처를 사용해서 오차 허용범위 지정할 수 있음
```java
import static org.hamcrest.number.IsCloseTo.*;

// ...
    assertThat(2.32 * 3, closeTo(6.96, 0.0005));
```

## 단언 설명

- 모든 JUnit 단언의 형식에는 message 라는 선택적 첫 번째 인자가 존재함. 
- 이는 첫 번째 인자로 **string** 을 넘겨주면 되는 것 같음.
- 이런 메세지들은 깔끔한 코드를 작성하는 것과 상충되는 Trade-off 관계에 있음
- 메세지에는 에러, 실패했을 때 이유를 바로 알 수 있어서 오류 파악에 직관적
- 없을 때
![image](https://user-images.githubusercontent.com/49026408/133898410-b3005355-cc86-4f4f-8960-69b71a82aed8.png)
- 있을 때
![image](https://user-images.githubusercontent.com/49026408/133898431-45ef1117-afee-48b1-96a8-f0c6628398ca.png)
(굳이 코드로 찾아가지 않아도 뭐가 에러인지 바로 알 수 있을 것 같긴함. 취향차이...)

## 예외 처리 방법

- `@Test(expected=예외)` 와 같이 Annotation 사용
  - 예외가 발생하지 않으면 실패임
- try/catch 와 fail
  - `org.junit.Assert.fail()` 을 try 문 안에 넣어서 예외가 발생하지 않으면 fail() 발생시킴
  - catch 조건부에 expected 를 명시해 코드 리더에게 예상했음을 표현할 수 있음
- ExpectedException 규칙
  - `InsufficientFundsException` 는 `RuntimeException` 을 상속함
```java
@Rule
   public ExpectedException thrown = ExpectedException.none();  
   
 @Test
 public void exceptionRule() {
    thrown.expect(InsufficientFundsException.class); 
    thrown.expectMessage("balance only 0");  
    
    account.withdraw(100);  
 }
```
- 이 밖에도 여러 방법이 있음
- 되도록 throws 를 이용해 예외를 다루는 것을 추천!

## 정리하자면

- 햄크레스트 매처를 이용하면
  - 객체 타입을 검사할 수 있다
  - 두 객체의 참조가 같은 인스턴스인지 검사할 수 있다
  - 다수의 매처를 결합해 AND, OR 논리로 검사를 진행할 수 있다
  - 컬렉션을 검사할 수 있다
- 이외 필요한 기능은 햄크레스트 API 문서를 참조할 것!

## Questions

- iloveyouboss_13/test/scratch/AssertTest.java
  - scratch 가 뭐지
  - AssertTest 클래스 안에 Account, Customer 를 외부에서 source 만들어서 사용해도 괜찮겠지?
  - @Ignore 데코레이터 실제 많이 쓰이는지?
  - JUnit 단언으로 message 넘겨주려면 그냥 string type 을 첫 번째에 넣으면 될까?
- 코드의 행복한 경로가 뭐지?
- 관점 지향 프로그래밍?