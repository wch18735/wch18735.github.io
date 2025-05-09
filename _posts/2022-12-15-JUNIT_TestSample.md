---
title: "[Junit] JUnit5 Test Code 실습"
excerpt: "junit5 test code practice"
date: 2022-12-15
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

# 객체지향 패러다임 개발을 위한 테스트 코드 실습

테스트 코드를 활용하면 조금 더 수월하게 객체지향 패러다임 개발을 수행할 수 있다. 뿐만 아니라 잘 작성된 테스트 코드는 그 자체적으로 정돈된 문서처럼 활용될 수 있으며, 코드 결함을 더욱 쉽게 발견할 수 있도록 도와주어 결과적으로 개발 시간을 단축시켜준다. 또한 리팩토링 시 안정성을 확보해 개발자가 자유롭게 수정 가능한 환경을 제공하며, 테스트 지향 코드 작성을 통해 조금 더 낮은 결합돌르 가진 설계를 얻을 수 있다.

이러한 개발 방법론을 이제는 너무나 유명한 TDD (Test Driven Development)라고 하며, 지금은 선택이 아닌 개발자의 필수 소양으로서 자리 잡혀가고 있다. 물론 프로덕션 코드보다 테스트 코드를 먼저 작성한다라는 TDD의 정의가 항상 실현될 수는 없다. 그러나 결과적으로 Test Coverage 100%를 목표하다보면 자연스레 소프트웨어 품질은 이를 따라올 것이다.

## 요구사항

테스트 코드 실습 요구사항은 아래와 같다.

```java
/*
* 요구사항
* 비밀번호는 최소 8자 이상 12자 이하여야 한다.
* 비밀번호가 8자 미만 또는 12자 초과인 경우 IllegalArgumentException 예외를 발생시킨다.
* 경계조건에 대해 테스트 코드를 작성해야 한다.
*/
```

이를 위해 기본적으로 아래와 같이 dependencies를 추가해준다. Gradle 프로젝트를 생성하면 기본적으로 제공되는 JUnit5는 Java 단위 테스트 프레임워크이다. 반면 사용자가 직접 추가해줘야 하는 라이브러리도 있는데, 그 중 하나인 AssertJ는 테스트 코드 가독성을 높여주는 자바 라이브러리이다.

```gradle
plugins {
    id 'java'
}

group 'org.example'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.passay:passay:1.6.1'

    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'
    testImplementation 'org.junit.jupiter:junit-jupiter-params:5.8.2'

    implementation 'org.assertj:assertj-core:3.23.1'
}

test {
    useJUnitPlatform()
}
```

## 테스트 구현을 통한 더 낮은 결합도 획득

많은 개발자들이 올바른 테스트 구현을 위해 최대한 Production 코드와 동일한 패키지 구조를 가져가는 것을 추천한다. 그러나 동시에 테스트 코드와 프로덕션 코드가 정확히 1:1 대응이 되는 것을 경계하라고 한다. 아래는 테스트 코드를 통해 더 낮은 결합도를 가진 설계를 얻은 것이다.

```java
package org.example;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

import static org.assertj.core.api.Assertions.assertThatCode;

/*
* 요구사항
* 비밀번호는 최소 8자 이상 12자 이하여야 한다.
* 비밀번호가 8자 미만 또는 12자 초과인 경우 IllegalArgumentException 예외를 발생시킨다.
* 경계조건에 대해 테스트 코드를 작성해야 한다.
*/
public class PasswordValidatorTest {

    @DisplayName("비밀번호가 최소 8자 이상, 12자 이하면 예외가 발생하지 않는다")
    @Test
    void validatePasswordLengthTest() {
        assertThatCode(()-> PasswordValidator.validate("sample-account~!"))
                .doesNotThrowAnyException();
    }

    @DisplayName("비밀번호가 8자 미만 또는 12자 초과하는 경우 IllegalArgumentException 에러가 발생한다")
    @ParameterizedTest
    @ValueSource(strings = {"1234567", "1234567890123"})
    void validatePasswordLengthTest_IllegalArgumentException(String password) {
        assertThatCode(()-> PasswordValidator.validate(password))
                .isInstanceOf(IllegalArgumentException.class)
                .hasMessage(PasswordValidator.WRONG_PASSWORD_LENGTH_EXCEPTION_MESSAGE);
    }

    //***********************************************************************//
    /* 테스트하기 쉬운 코드를 작성하려고 노력하면 더 낮은 결합도를 가진 설계를 얻을 수 있다 */
    //***********************************************************************//

    @DisplayName("비밀번호가 최소 8자 이상, 12자 이하면 예외가 발생하지 않는다.")
    @Test
    void validatePasswordTest_by_CorrectPasswordGenerator() {
        // given
        PasswordValidator passwordValidator = new PasswordValidator();

        // when, then
        assertThatCode(() -> passwordValidator.validate_by_password_generator(new CorrectPasswordGenerator()))
                .doesNotThrowAnyException();
    }

    @DisplayName("비밀번호가 8자 미만 또는 12자 초과하는 경우 IllegalArgumentException 예외가 발생한다.")
    @Test
    void validatePasswordTest_by_WrongPasswordGenerator() {
        // given
        PasswordValidator passwordValidator = new PasswordValidator();

        // when, then
        assertThatCode(() -> passwordValidator.validate_by_password_generator(new WrongPasswordGenerator()))
                .isInstanceOf(IllegalArgumentException.class)
                .hasMessage(PasswordValidator.WRONG_PASSWORD_LENGTH_EXCEPTION_MESSAGE);
    }
}
```

기존 PasswordValidator 구조에서는 임의로 생성되는 문자열 길이를 알지 못 해 테스트 결과가 일관적이지 않았다. 그렇기 때문에 해당 테스트에 일관성을 부여하기 위해 validatePasswordTest_by_CorrectPasswordGenerator()와 validatePasswordTest_by_WrongPasswordGenerator() 두 테스트를 작성하게 되었는데, 이 과정에서 정적 변수(static variable)로 사용하던 RandomPasswordGenerator를 외부에서 주입받는 형태로 리팩토링하게 된다.

<img src="/_img/2022-12-15/loose coupling example.png">

오래된 대기업의 사내 시스템을 보면 10년이 넘어가는 레거시 시스템들은 10년 전 작성되어 유지되어 오는 코드들이다. 사실 10년 전 코드에서 Unit test를 구경하기란 하늘에 있는 별 따기와 마찬가지라고 생각한다. 그치만 포기하면 안 된다. 

> "우리 TDD인가 MSA인가 뭔가가 하여튼 좋다더라. 적용해" 라는 명령이 떨어져 Top-down으로 일하게 된 개발 집단에게 밝은 미래를 기대하긴 힘들다.

Top-down으로 떨어지기 전에 Bottom-up으로 문화를 바꿔나가보려 노력해보는 것이 좋겠다. 엔지니어링은 멀지 않은 곳에 있다.