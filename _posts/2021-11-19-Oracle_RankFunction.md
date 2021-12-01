---
title: "[Oracle] 순위 함수"
excerpt: "oracle rank functions"
date: 2021-11-19
category:
    - oracle
tag:
    - database
    - oracle
    - sql
    - rank
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# 순위 함수

ORDER BY 절로 정렬된 값들에 INDEX 를 부여하고 싶을 때, 단순히 ROWNUM 을 사용해보자. 결과는 일련번호들이 뒤죽박죽으로 섞이게 된다. ROWNUM 은 WHERE 절에서 생성되는 것이고, ORDER BY 는 그 이후에 사용되기 때문이다.

정렬된 상태에 그대로 번호를 부여하는 함수가 있을까?

```sql
SELECT ROW_NUMBER() OVER (ORDER BY HIT), ID, TITLE, WRITER_ID, REGDATE, HIT
FROM NOTICE;
```

이렇게 `ROW_NUMBER() OVER` 를 사용한다. OVER 키워드는 HIT 으로 정렬한 후에 사용하라는 뜻이다. 여기서 `RANK() OVER` 로 바뀌면 순서가 아니라 순위표가 된다. 같은 점수로 생기는 같은 등수 이후에 순위를 이어나가고 싶다면 `DENSE_RANK()` 를 사용하면 된다.

끝으로 특정 기준별로 나눠서 정리하려면 OVER 뒤 괄호 안에 `PARTITION BY` 를 지정해 그룹별로 따로 순서, 순위를 정해줄 수 있다. `DESC` 또는 `ASC` 역시 사용자가 정할 수 있다.