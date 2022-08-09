---
title: "[Python] Byte calculation"
excerpt: "정규식(regular expression)을 이용한 바이트 계산"
date: 2022-08-09
layout: single
classes: wide
category:
    - python
tag:
    - regular expression
    - python
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 정규식(Regular Expression)을 이용한 바이트 계산

며칠 전부터 몇 건의 Interface Error 가 산발적으로 발생했다. 알아보니 Destination 시스템으로 정보를 보낼 때, 상대 시스템 테이블 컬럼 속성이 VARCHAR(100)으로 잡혀있어 100 Byte 가 넘는 문자열이 전송되지 않는 오류였다. Oracle DB를 사용하고 있어서 `LENGTHB()` 함수를 사용해 100 byte 보다 긴 문자열을 가진 행을 골라내는 건 어렵지 않을텐데, 파이썬으로도 짜볼 수 있지 않을까? 싶어서 작성해봤다.

```python
import re

if __name__ == "__main__":
    target_str = ""

    consonants = re.compile('[ㄱ-ㅎ]').findall(target_str)
    vowels = re.compile('[ㅏ-ㅣ]').findall(target_str)
    hanguel = re.compile('[가-힣]').findall(target_str)
    alphabets = re.compile('[a-z|A-Z]').findall(target_str)
    numbers = re.compile('\d').findall(target_str)
    speicals = re.compile('[^ A-Z|a-z|0-9|가-힣]').findall(target_str)
    spaces = re.compile('\s').findall(target_str)


    print(len(consonants), len(vowels), len(hanguel), len(speicals), len(alphabets), len(numbers), len(spaces))
    byte_num = len(consonants) * 3 + \
               len(vowels) * 3 + \
               len(hanguel) * 3 + \
               len(speicals) * 3 +\
               len(alphabets) + \
               len(numbers) + \
               len(spaces)

    print(byte_num)
```

생각보다 고려할 요소가 많았다. 한글 자음, 한글 모음, 한글, 알파벳, 특수문자, 그리고 생각하지 못 했던 공백까지! 꽤 재미있었다. 곧, Python 으로 Front-end 앱도 짤 수 있다고 하는데 앞단에서 이렇게 짜놓은 Snippet code 들을 익숙하게 찍어 넣을 수 있다면 아주 편리할 것 같다는 생각을 했다.