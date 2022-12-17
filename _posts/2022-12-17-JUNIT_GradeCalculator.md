---
title: "[Junit] TDD로 학점 계산기 구현하기"
excerpt: "grade calculator test sample"
date: 2022-12-17
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

# TDD로 학점 게산기 구현하기

강의에서 보았던 객체지향 설계 및 구현 프로세스를 정리한 내용인데, 생각보다 많은 내용을 내포하고 있는 것 같다. 여기에 한 가지 추가하고 싶은 내용은 필요 시 특정 단계로 돌아가 Iteration을 반복함으로써 소프트웨어 품질을 높일 수 있다는 것이다. 이번 학점 계산기 구현하기에서는 4번과 5번 책임 할당을 주로 다뤘다.

1. 도메인을 구성하는 객체에는 어떤 것들이 있는지 나열
2. 객체들의 관계를 고민
3. 동적인 객체를 정적인 타입으로 추상화해서 도메인 모델링
4. 협력 과정을 통한 책임 할당 및 설계
5. 객체들을 포괄하는 타입에 책임 할당
6. 설계 기반 구현

## 테스트 코드

먼저 안전하게 리팩토링을 수행하기 위한 테스트 코드를 작성한다. 이 테스트를 통과시키는 코드를 빠르게 작성하고, 리팩토링을 수행한다.

```java
public class GradeCalculatorTest {
    private static List<Course> courses = List.of(
            new Course("OOP", 3, "A+"),
            new Course("Computer Science", 3, "A+")
    );

    @DisplayName("평균 학점을 계산한다")
    @Test
    void calculateAverageGradeTest() {
        GradeCalculator gradeCalculator = new GradeCalculator(courses);

        double gradeResult = gradeCalculator.calculateResult();
        assertThat(gradeResult).isEqualTo(4.5);
    }
}
```

## 초기 GradeCalculator

초기 GradeCalculator 모습은 아래와 같다. calculateResult() 메소드를 통해 현재 가지고 있는 강의 정보 리스트에서 최종 점수를 산출해낸다. 여기서는 두 가지 Refactoring을 진행해 볼 수 있다.

```java
public class GradeCalculator {
    private List<Course> courses;

    public GradeCalculator(List<Course> courses) {
        this.courses = courses;
    }

    public double calculateResult() {
        double resultGrade = 0.0;

        // 학점 수 x 교과목 평점
        for(Course course: courses){
            resultGrade += course.getCredit() * course.getGradeToDouble();
        }

        // 수강신청 총 학점 수
        int totalCompletedCredit = courses.stream()
                .mapToInt(Course::getCredit)
                .sum();

        return resultGrade / totalCompletedCredit;
    }
}
```

## getter() → 기능 위임

먼저 getter()를 통해 직접 정보를 조회해 <u>누적 학점 수 * 교과목 평점을 구하던 책임을 Course에 위임</u> 할 수 있다. 만약 누적 학점 수 * 교과목 평점 정보가 많은 곳에서 활용된다면 해당 위치에서 위 코드를 복사 및 붙여넣기하는 불필요한 과정을 겪어야 한다. 거기까지는 괜찮으나, 전체 점수를 구하는 로직이 변경되는 순간 흩어진 코드들 하나하나를 찾아 수정해줘야 하는 일까지 진행해야 한다.

이러한 이유로 Course에 해당 로직을 녹이고, 아래와 같이 수정해준다.

```java
public class GradeCalculator {
    private List<Course> courses;

    public GradeCalculator(List<Course> courses) {
        this.courses = courses;
    }

    public double calculateResult() {
        double resultGrade = 0.0;

        // 학점 수 x 교과목 평점
        for(Course course: courses){
            resultGrade += course.multiplyCreditAndGrade(); // getter() → 메소드
        }

        // 수강신청 총 학점 수
        int totalCompletedCredit = courses.stream()
                .mapToInt(Course::getCredit)
                .sum();

        return resultGrade / totalCompletedCredit;
    }
}
```

## 일급 컬렉션 사용

일급 컬렉션이란 클래스 내부에 유일한 Attribute가 컬렉션인 클래스를 말한다. 이를 활용하면 조회 로직을 일부 캡슐화 할 수 있고, 로직 변경 시 수정을 최소화 할 수 있다. 먼저 Course 리스트 일급 클래스를 작성한다.

```java
public class Courses {
    List<Course> courses;
    public Courses(List<Course> courses) {
        this.courses = courses;
    }
}
```

그렇게 되면 이제 calculateResult() 메소드에서 사용했던 **계산 로직**을 일급 클래스에 위임 할 수 있게 된다. 이렇게 일급 클래스에 권한을 위임하게 되면 코드는 아래와 같이 간결하게 바뀌게 된다. 그리고 이런 위임을 이용한 리팩토링을 통해 추후 코드 변경에 유연하게 대응 할 수 있는 설계를 반영 할 수 있다. 

```java
public class GradeCalculator {
    private Courses courses;
    public GradeCalculator(List<Course> courses) {
        this.courses = new Courses(courses);
    }

    public double calculateResult() {
        // 학점 수 x 교과목 평점
        double resultGrade = courses.getGradeResult();

        // 수강신청 총 학점 수
        int totalCompletedCredit = courses.getTotalCompletedCredit();

        return resultGrade / totalCompletedCredit;
    }
}
```