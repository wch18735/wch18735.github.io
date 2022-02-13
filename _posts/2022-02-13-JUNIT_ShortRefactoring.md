---
title: "[Junit] Short Refactoring criteria"
excerpt: "short refactoring criteria"
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

# 깔끔한 코드로 리팩토링하기

시스템이 커지면 메서드 유틸리티, 클래스 등의 중복이 발생 할 수 있음. 중복되거나 꼬인 코드 조각들은 유지 보수 비용이 증가하고, 변경에 대한 리스크도 함께 늘어남. 따라서 시스템에 있는 중복의 양을 최소화 해야 함.

낮은 중복성과 높은 명확성 두 가지를 만족하면 높은 투자 수익률(ROI) 달성 가능. 단위 테스트는 이를 만족시킬 수 있는 훌륭한 도구로서 사용할 수 있음.

## 작은 단위 리팩토링

리팩토링의 정의는 다음과 같음.
- **리팩토링:** 기존 기능은 그대로 유지하며 코드 하부 구조를 건강하게 변형하는 것
- 코드 구조를 바꾸어도 이전의 정상 동작을 보장함을 뜻 함

리팩토링 방법
- 메서드 추출
ex) 168p `matches()` 메서드의 복잡도를 줄여 코드가 무엇을 담당하는지를 쉽게 이해할 수 있게 하는 것. 

```java
// 변화 전
public boolean matches(Criteria criteria) {
      score = 0;
      
      boolean kill = false;
      boolean anyMatches = false;
      for (Criterion criterion: criteria) {
         Answer answer = answers.get(
               criterion.getAnswer().getQuestionText());
         boolean match = 
               criterion.getWeight() == Weight.DontCare ||
               answer.match(criterion.getAnswer());
         if (!match && criterion.getWeight() == Weight.MustMatch) {
            kill = true;
         }
         if (match) {
            score += criterion.getWeight().getValue();
         }
         anyMatches |= match;
         // ...
      }
      if (kill)
         return false;
      return anyMatches;
   }
```
↓
```java
// 변화 후
public boolean matches(Criteria criteria) {
      score = 0;
      
      boolean kill = false;
      boolean anyMatches = false;
      for (Criterion criterion: criteria) {
         Answer answer = answers.get(
               criterion.getAnswer().getQuestionText());
         boolean match = matches(criterion, answer); // changed
         if (!match && criterion.getWeight() == Weight.MustMatch) {
            kill = true;
         }
         if (match) {
            score += criterion.getWeight().getValue();
         }
         anyMatches |= match;
         // ...
      }
      if (kill)
         return false;
      return anyMatches;
   }

private boolean matches(Criterion criterion, Answer answer) {
      return criterion.getWeight() == Weight.DontCare || answer.match(criterion.getAnswer());
```

단위 테스트가 촘촘하고 안전 지향적일 수록 위와 같은 변화에도 차분하게 이를 적용할 수 있음

## 메서드 집합화

- 위에서 작업한 `boolean match = matches(criterion, answer);` 는 **Profile 객체와 관계 없음.** 이를 조금 더 관계 깊은 **Answer**나 **Criterion** 객체 메서드로 옮김. 예시로 위의 코드는 `boolean match = crietrion.matches(answer);` 로 고칠 수 있음.

- **디메테르 법칙 (the Law of Demeter):** 다른 객체로 전파되는 연쇄적인 메서드 호출을 피해야 한다는 법칙
`Answer answer = answers.get(criterion.getAnswer().getQuestionText());` 에서 연쇄 호출을 발견할 수 있음

- 위 코드에서 `criterion` 객체를 전달해 scope 를 정해주는 방식으로 추출할 수 있음
```java
private Answer answerMatching(Criterion criterion) {
     return answers.get(criterion.getAnswer().getQuestionText());
}
```

## 자동화된 리팩토링과 과도한 리팩토링

- Eclipse IDE에서 제공하는 리팩토링 모듈을 사용해보기
- 솔직히 이 얘기가 갑자기 왜 나오는지 모르겠음 (유동환님은 그냥 프로그래머만 하셨으면 좋겠다)
- 리팩토링한 결과로 세 번의 반복문이 형성되는 경우
   - 장점: 즉시 이해할 수 있을 정도로 알고리즘 깔끔해짐.
   - 단점: 데이터가 엄청나게 많아질 경우 요구 성능을 만족하지 못 할 수 있음
- 결론: **성능이 문제되지 않는다면 가독성이 높은 상태로 유지하라**

## 느낀점
- 진짜 번역... 파파고가 책 읽어주면 딱 이 느낌일 듯
- 리팩토링 3판 (Javascript 기반 리팩토링 기법) 책 있는데 필요한 사람 얘기해!