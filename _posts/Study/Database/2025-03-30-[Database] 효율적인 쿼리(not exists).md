---
title: "[Database] 효율적인 쿼리에 대한 고민(not exists)"
author: baduk
date: 2025-03-30 15:18:00 +0900
categories: [Study, Database]
tags: [Database]
---

## 시나리오
A테이블과 B테이블은 특정 칼럼(ex. video_id)을 통해 조인이 가능하다고 해보자.

그런데 A테이블에는 있지만 B테이블에는 없는 특정 칼럼(ex. video_id)를 찾고 싶을 때, 쿼리를 작성하는 방법은 간단하게 아래 두 가지가 있을 것이다.

## 1. NOT EXISTS 사용
```sql
SELECT *
FROM main_table mt
WHERE NOT EXISTS (
    SELECT 1
    FROM table_A a
    WHERE a.video_id = mt.video_id
);
```

## 2. LEFT JOIN + IS NULL 사용
```sql
SELECT mt.*
FROM main_table mt
LEFT JOIN table_A a ON mt.video_id = a.video_id
WHERE a.video_id IS NULL;
```

**1.NOT EXISTS 사용 작동 플로우**
mt 테이블의 모든 행을 하나씩 가져오면서 a 테이블에 같은 video_id가 있는지 확인하고,
없으면 -> Not Exists 이므로 해당 video_id에 대한 mt 테이블의 로우를 셀렉트한다.

**2.LEFT JOIN + IS NULL 사용 작동 플로우**
mt 테이블을 기준으로 video_id로 a테이블을 조인시키고, 이후 video_id 가 null인것 즉, a테이블에는 존재하지 않는 video_id를 필터링 시킨다.

1번의 경우 a 테이블에서 video_id를 조회하니, 당연히 a 테이블의 video_id에 인덱스가 잘 걸려있으면 1번이 일반적으로 빠를것이다.

뭐가 더 효율적인지 정확히 파악하려면 EXPLAIN ANALYZE로 실행 계획을 확인해야 될 것이다.