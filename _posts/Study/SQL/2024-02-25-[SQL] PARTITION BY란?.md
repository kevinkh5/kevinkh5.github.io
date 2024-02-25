---
title: '[SQL] PARTITION BY란?'
author: baduk
date: 2024-02-25 18:54:00 +0900
categories: [Study, SQL]
tags: [SQL]
---

## Partition by란?

PARTITION BY를 사용하면 특정 칼럼에 대해서 그룹화하여 집계함수를 적용시킬 수 있다. PARTITION BY는 마치 GROUP BY와 비슷한 역할을 한다.

예를들어 SUM 집계함수를 사용한 예는 아래와 같다.
```sql
SELECT *, SUM(S.SCORE) OVER (PARTITION BY S.CLASS) AS SCORE_SUM
FROM STUDEN_TABLE S
```
위와 같이 작성할 경우 CLASS별로 나누어서 SCORE에 대해 SUM값을 구할 수 있다.

이외에 AVG, MAX, MIN 등이 사용가능하다.