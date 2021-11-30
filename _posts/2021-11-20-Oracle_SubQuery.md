---
title: "[Oracle] 서브 쿼리란"
excerpt: "sub-query"
date: 2021-11-20
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

# 서브 쿼리 (Sub-Query)

만약 `1) 정렬한 다음`` 2) 그 중 상위 10개` 를 뽑는 쿼리를 작성해보자. 아래와 같이 요구사항을 그대로 따르는 질의문을 작성하고 실행하면 오류가 발생한다.

```sql
SELECT * FROM MEMBER
ORDER BY REGDATE                -- 주어진 테이블을 정렬
WHERE ROWNUM BETWEEN 1 AND 10;  -- 정렬된 테이블에서 상위 10개
```

위 쿼리는 순서를 지키지 않았다. 그러나 이처럼 명령어 순서가 논리적으로 바뀌어야 하는 경우 사용할 수 있는 것이 **서브 쿼리** 이다.

```sql
SELECT * FROM (SELECT * FROM MEMBER ORDER BY REGDATE)
WHERE ROWNUM BETWEEN  1 AND 10;
```

소괄호 `()` 로 둘러싸인 쿼리를 서브 쿼리라고 한다. 수식에서도 **먼저** 계산하는 것을 소괄호로 묶는 것처럼 쿼리 문장 안에서도 먼저 만들 격자형 데이터에 소괄호를 묶어주면 된다.

