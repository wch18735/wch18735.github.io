---
title: "[Junit] TDD로 사칙연산 계산기 구현하기"
excerpt: "operator test sample"
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

# TDD로 사칙연산 계산기 구현하기

TDD는 아래 Iteration을 지속적으로 반복하며 소프트웨어를 구축하는 것이다. 이 때, 핵심은 **일단**이라는 마인드다.

<img src="/_img/2022-12-17/TDD_Graphic.jpg">

**일단** Test를 작성하고, Failure가 나오면 **일단** Passes를 띄우고, **일단** Refactoring 하고, Failure가 나오면 다시 처음으로 돌아가 같은 과정을 반복한다. 이렇게 지속적인 반복을 통해 점진적으로 소프트웨어를 구축해 나간다.

## Calculator 요구사항

먼저, Calculator 요구사항은 아래와 같다.

- 간단한 사칙연산을 할 수 있다.
- 양수로만 계산할 수 있다.
- 나눗셈에서 0을 나누는 경우 IllegalArgument 예외를 발생시킨다.
- MVC패턴(Model-View-Controller) 기반으로 구현한다.

## calculate() 메소드 작성 후 테스트

Calculator 계산기는 calculate() 메소드로 operand1, operand2와 operator를 매개변수를 전달받는다. 문자열 형태로 전달받은 operator의 형태에 따라 서로 다른 기능으로 동작한다.

```java
public class Calculator {
    public static int calculate(PositiveNumber operand1, String operator, PositiveNumber operand2) {    
        if(operator == "+"){
            return operand1 + operand2;
        } else if(operator == "-"){
            return operand1 - operand2;
        } else if(operator == "*"){
            return operand1 * operand2;
        } else {
            return operand1 / operand2;
        }
        
    }
}
```

이렇게 작성한 클래스를 테스트해보자. test 아래 동일한 패키지 구조를 구축하고, 같은 위치에 CalculatorTest 클래스를 작성한다.

```java
public class CalculatorTest {
    @DisplayName("Calculator 클래스를 테스트한다")
    @ParameterizedTest
    @MethodSource("formulaAndResult")
    void calculateTest(int operand1, String operator, int operand2, int result) {
        int calculateResult = Calculator.calculate(new PositiveNumber(operand1), operator, new PositiveNumber(operand2));
        assertThat(calculateResult).isEqualTo(result);
    }

    private static Stream<Arguments> formulaAndResult(){
        return Stream.of(
            arguments(1, "+", 2, 3),
            arguments(1, "-", 2, -1),
            arguments(4, "*", 2, 8),
            arguments(4, "/", 2, 2)
        );
    }
}
```

여기서 사용된 **@ParameterizedTest** 어노테이션은 메소드가 매개변수가 전달되는 테스트라는 것을 나타낸다. 테스트 작성자가 @ParameterizedTest를 사용하기 위해서는 반드시 한 개 이상의 ArgumentProvider를 특정해야 한다. 실제로 전달되는 인자는 @ArgumentSource 어노테이션이나 대응시키는 Annotation을 통해 지정할 수 있다.

또한, 테스트 작성자는 @ParameterizedTest로 메소드를 호출을 통해 전달되는 인자가 Stream 형태로 전달되도록 작성해야 한다. 가령 위 예시에서는 **@MethodSource** 어노테이션 안에 Stream\<Argument\>를 반환하는 formulaAndResult 메소드를 실제 인자로 지정했다.

**org.junit.jupiter.params.provider.Arguments**를 통해 전달되는 Arguments interface는 접근성을 제공하는데 사용된다. @ParameterizedTest 메소드에서 사용되는 인자들이 메소드의 매개변수에 대입되도록 하려면, 그리고 그런 인자들이 여러 배열로 존재할 때 Arguments 배열을 Stream으로 제공해 순회하도록 한다.

## calculate() 리팩토링

테스트를 통과시키고 나면 마음껏 리팩토링을 할 수 있다. 일종의 안정감이 생긴다. if-else 분기를 통한 코드는 calculate()에서만 사용할 수 있으며, 비슷한 기능을 동작시키기 위해서는 if-else로 작성된 부분을 복사해 붙여 넣는 번거로운 과정을 거쳐야 한다.

이를 막기 위해 사칙연산 기호와 대응되는 Enum을 아래와 같이 작성하며, 양수로만 계산을 수행할 수 있다는 요구사항을 적용하기 위한 ValueObject PositivieNumber를 선언한다.

```java
public enum ArithmeticOperator {
    ADDITION("+"){
        @Override
        public int arithmeticCalculate(PositiveNumber operand1, PositiveNumber operand2) {
            return operand1.toInt() + operand2.toInt();
        }
    }, SUBTRACTION("-"){
        @Override
        public int arithmeticCalculate(PositiveNumber operand1, PositiveNumber operand2) {
            return operand1.toInt() - operand2.toInt();
        }
    }, MULTIPLICATION("*"){
        @Override
        public int arithmeticCalculate(PositiveNumber operand1, PositiveNumber operand2) {
            return operand1.toInt() * operand2.toInt();
        }
    }, DIVISION("/"){
        @Override
        public int arithmeticCalculate(PositiveNumber operand1, PositiveNumber operand2) {
            return operand1.toInt() / operand2.toInt();
        }
    };

    private final String _operator;

    ArithmeticOperator(String operator) {
        this._operator = operator;
    }

    public abstract int arithmeticCalculate(final PositiveNumber operand1, final PositiveNumber operand2);

    public static int calculate(PositiveNumber operand1, String operator, PositiveNumber operand2){
        ArithmeticOperator arithmeticOperator = Arrays.stream(values())
                .filter(v -> v._operator.equals(operator))
                .findFirst()
                .orElseThrow(() -> new IllegalArgumentException("올바른 사칙연산이 아닙니다"));

        return arithmeticOperator.arithmeticCalculate(operand1, operand2);
    }
}
```