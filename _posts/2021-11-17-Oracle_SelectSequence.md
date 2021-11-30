---
title: "[Oracle] SELECT 구절"
excerpt: "select sequence and order by"
date: 2021-11-17
category:
    - oracle
tag:
    - database
    - oracle
    - sql
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# SELECT 구절

`SELECT`, `FROM`, `WHERE`, `GROUB BY`, `HAVING`, `ORDER BY`

위에 나열된 순서를 꼭 외워야 한다.

- **FROM :** 격자형 데이터를 만든다
- **WEHRE :** 조건에 맞는 행 선별
- **GROUB BY :** SUM, COUNT, AVG, MIN, MAX 등의 집계 함수 사용
- **HAVING :** 집계된 내용을 필터링 할 때 사용
- **ORDER BY :** 최종적인 값을 정렬

## 정렬하기

정렬 순서는 크게 `ASC` 과 `DESC` 방식이 있다.

ORDER BY 와 함께 사용된다.

명시하지 않으면 기본적으로 오름차순 정렬이 된다.

```sql
SELECT * FROM NOTICE ORDER BY HIT DESC;
```

## 집계하기

`WHERE` 절로 조건에 부합하는 격자 행에 대한 집계를 수행할 수 있다.

예를 들어, 총 레코드 수를 구할 수 있다.

이때, COUNT 는 NULL 을 세지 않기 때문에 NULL 이 포함될 수 있는 COLUMN 은 지정하지 않는 것이 바람직하다.

아래는 집계 함수를 사용하는 예시이다.

```sql
SELECT SUM(HIT) FROM NOTICE;
SELECT AVG(HIT) FROM NOTICE;

SELECT WRITER_ID, COUNT(ID) "COUNT" 
FROM NOTICE 
GROUP BY WRITTER_ID
HAVING COUNT(ID) >= 5 -- WHERE 절에서 사용할 수 없기 대문에 HAVING 에서 사용
ORDER BY COUNT(ID) DESC;
```

함수 실행 순서는 아래와 같다.

`FROM → CONNECT BY → WHERE → GROUP BY → HAVING → SELECT → ORDER BY`

SELECT 에서 사용한 별칭은 그렇기에 GROUP BY 나 WHERE 에서는 사용할 수 없다.