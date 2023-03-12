---
title: "[Coding Test] Bit Operation in cpp"
excerpt: "bit operation practice in cpp"
date: 2023-03-12
layout: single
classes: wide
category:
    - coding test
tag:
    - cpp
    - bit operation
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Bit operation

비트 연산(Bit operation)은 컴퓨터에서 데이터를 처리하는 기술 중 하나로, 2진수 숫자의 각 비트(bit)에 대해 수행되는 연산이다. 비트 연산은 AND(&), OR(\|), XOR(^), NOT(~)과 같은 논리 연산자와 시프트(Shift) 연산자를 사용한다.

비트 연산은 주로 컴퓨터의 하드웨어적인 부분에서 사용되며, 빠른 연산 속도와 간결한 코드로 인해 많이 사용된다. 이를테면, 마이크로프로세서의 레지스터 조작, 암호화/복호화, 이미지 처리, 네트워크 프로토콜 등에서 사용된다.

아래는 주요한 비트 연산들에 대한 설명이다.

**1. AND 연산(&):** 두 비트의 값이 모두 1인 경우에만 결과값이 1이 되며, 다른 경우에는 결과값이 0이 된다.

&nbsp;&nbsp;예시: 1010 & 1100 = 1000

**2. OR 연산(\|):** 두 비트 중 하나라도 1이면 결과값이 1이 되며, 두 비트가 모두 0인 경우에만 결과값이 0이 된다.

&nbsp;&nbsp;예시: 1010 \| 1100 = 1110

**3. XOR 연산(^):** 두 비트의 값이 서로 다른 경우에만 결과값이 1이 되며, 두 비트가 같은 경우에는 결과값이 0이 된다.

&nbsp;&nbsp;예시: 1010 ^ 1100 = 0110

**4. NOT 연산(~):** 비트의 값이 0인 경우에는 1로, 1인 경우에는 0으로 바꾼다.

&nbsp;&nbsp;예시: ~1010 = 0101

**5. 시프트 연산:** 비트들을 지정한 만큼 좌측 또는 우측으로 이동시키며, 이동한 비트는 버려지며, 빈 자리는 0으로 채워진다.

&nbsp;&nbsp;&nbsp;&nbsp;좌측 시프트(<<) : 비트를 왼쪽으로 이동시키는 연산.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;예시: 1010 << 2 = 101000

&nbsp;&nbsp;&nbsp;&nbsp;우측 시프트(>>) : 비트를 오른쪽으로 이동시키는 연산.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;예시: 1010 >> 2 = 0010

비트 연산은 이진수(binary)에서 작업을 수행하기 때문에, 10진수(decimal)나 16진수(hexadecimal)와 같은 다른 진법으로 표현된 숫자를 이진수로 변환하여 사용해야 한다.

## #include\<bitset\>

`bitset`은 C++ 표준 라이브러리에서 제공하는 비트 단위(bit-level) 연산을 수행하기 위한 클래스이다. 이 클래스는 고정 크기의 이진 비트 배열을 표현한다. bitset 객체는 다음과 같이 선언할 수 있다.

```cpp
#include <bitset>

std::bitset<크기> 비트셋;
```

여기서 크기는 비트셋의 크기를 나타낸다. 예를 들어, 8개의 비트를 가지는 bitset 객체를 선언하려면 다음과 같이 작성할 수 있다.

```cpp
std::bitset<8> bits;
```

bitset 클래스는 다음과 같은 주요 멤버 함수를 제공한다.

- bitset::set(pos, val): 비트셋의 pos 위치에 값을 val로 설정한다.
- bitset::reset(pos): 비트셋의 pos 위치를 0으로 설정한다.
- bitset::flip(pos): 비트셋의 pos 위치의 값을 뒤집는다.
- bitset::count(): 비트셋에서 1의 개수를 반환한다.
- bitset::to_ulong(): 비트셋을 unsigned long 타입으로 변환한다.
- bitset::to_ullong(): 비트셋을 unsigned long long 타입으로 변환한다.

또한, bitset 클래스는 비트 단위(bit-level) 연산자인 AND(&), OR(\|), XOR(^), NOT(~) 연산자를 지원한다. 이 연산자를 이용하여 비트셋 간의 연산을 수행할 수 있다. 다음은 bitset 클래스를 이용하여 이진수 연산을 수행하는 예시 코드이다.

```cpp
#include <iostream>
#include <bitset>

int main() {
    std::bitset<8> x("00001111");
    std::bitset<8> y("01010101");
    
    std::bitset<8> z = x & y; // AND 연산
    std::cout << z << std::endl;
    
    z = x | y; // OR 연산
    std::cout << z << std::endl;
    
    z = x ^ y; // XOR 연산
    std::cout << z << std::endl;
    
    z = ~x; // NOT 연산
    std::cout << z << std::endl;
    
    return 0;
}
```

위 코드는 다음과 같은 출력을 생성한다.

```cpp
00000101
01001111
01011010
11110000
```