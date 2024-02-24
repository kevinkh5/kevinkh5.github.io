---
title: '[JOIN] SQL Join 연습2'
author: baduk
date: 2024-02-12 18:49:00 +0900
categories: [Study, SQL]
tags: [SQL]
---

# 서브쿼리 사용해서 특정조건 배제시키기
WHERE ~ NOT IN(
    SELECT ~
    FROM ~
    WHERE ~
) 

## 특정 기간동안 대여 가능한 자동차들의 대여비용 구하기
<https://school.programmers.co.kr/learn/courses/30/lessons/157339>
```sql
SELECT C.CAR_ID, C.CAR_TYPE, ROUND(C.DAILY_FEE*((100-P.DISCOUNT_RATE)/100)*30,0) AS FEE
FROM CAR_RENTAL_COMPANY_CAR C
INNER JOIN CAR_RENTAL_COMPANY_RENTAL_HISTORY H ON C.CAR_ID = H.CAR_ID
INNER JOIN CAR_RENTAL_COMPANY_DISCOUNT_PLAN P ON C.CAR_TYPE = P.CAR_TYPE
WHERE C.CAR_ID NOT IN (
SELECT H.CAR_ID
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY H
WHERE H.END_DATE >= '2022-11-01' AND H.START_DATE <= '2022-11-30')
AND C.CAR_TYPE IN ('세단','SUV') AND P.DURATION_TYPE = '30일 이상'
AND ROUND(C.DAILY_FEE*((100-P.DISCOUNT_RATE)/100)*30,0) >= 500000
AND ROUND(C.DAILY_FEE*((100-P.DISCOUNT_RATE)/100)*30,0) < 2000000
GROUP BY C.CAR_ID, C.CAR_TYPE, FEE
ORDER BY FEE DESC, C.CAR_TYPE ASC, C.CAR_ID DESC
```
11월1일부터 11월30일 동안 렌탈할 수 있는 차를 구하기 위한 조건을 거는게 좀 까다로웠던 문제 

# 랭크 사용해서 가장 많은 리뷰를 작성한 회원의 리뷰 조회하기
```sql
SELECT
    고객ID,
    이름,
    매출액,
    RANK() OVER (ORDER BY 매출액 DESC) AS 매출액_순위
FROM
    고객;
```
## 그룹별 조건에 맞는 식당 목록 출력하기
<https://school.programmers.co.kr/learn/courses/30/lessons/131124>
```sql
SELECT B.MEMBER_NAME, R.REVIEW_TEXT, DATE_FORMAT(R.REVIEW_DATE,'%Y-%m-%d') AS REVIEW_DATE
FROM REST_REVIEW R
INNER JOIN( -- 이너조인이든 조인이든 같은 동작함
SELECT R.MEMBER_ID, M.MEMBER_NAME, RANK() OVER(ORDER BY CNT DESC) AS RANKING
FROM (
SELECT *, COUNT(MEMBER_ID) AS CNT
FROM REST_REVIEW
GROUP BY MEMBER_ID) AS R
JOIN MEMBER_PROFILE M ON R.MEMBER_ID = M.MEMBER_ID) B
ON B.MEMBER_ID = R.MEMBER_ID
WHERE RANKING = 1
ORDER BY R.REVIEW_DATE
```
먼저 리뷰테이블에서 멤버아이디를 그룹바이로 묶어서 카운트 뽑는다.(5~8번 라인) 그리고 카운트 뽑은 테이블을 이용해서 카운트를 디센딩으로 했을 때 Rank나타낼 수 있도록 새 테이블을 또 만든다.(4번 라인) 그리고 이렇게 만들어진 테이블을 멤버 프로필과 조인시키고, 랭킹이 1인 조건을 걸어서 조건에 맞춰 오더바이로 정렬한다. 나머지 출력되어야할 필드 설정해주면 끝이다.

# 디스팅크트 사용해서 유니크한 ID의 개수를 가져오고 그걸 이용해서 비율도 구해보기

## 상품을 구매한 회원 비율 구하기
<https://school.programmers.co.kr/learn/courses/30/lessons/131534>
```sql
SELECT YEAR(B.SALES_DATE) AS YEAR, MONTH(B.SALES_DATE) AS MONTH, COUNT(DISTINCT A.USER_ID) AS PUCHASED_USERS, ROUND(COUNT(DISTINCT A.USER_ID)/
(SELECT COUNT(A.USER_ID)
FROM USER_INFO A
WHERE YEAR(A.JOINED) = 2021),1) AS PUCHASED_RATIO
FROM USER_INFO A
JOIN ONLINE_SALE B
ON A.USER_ID = B.USER_ID
WHERE YEAR(A.JOINED) = 2021
GROUP BY YEAR, MONTH
ORDER BY YEAR ASC, MONTH ASC
```
1.2021년 전체 가입자 개수 가져오기

유저인포 테이블을 2021년 가입자 가져오도록 필터걸어서, 그 테이블의 개수를 가져온다

2.유저인포 테이블로 부터 그룹바이 걸어서 디스팅크트로 카운트세기(그룹별로 유니크하게 카운트세기)

유저인포 테이블과 온라인세일 테이블을 유저아이디로 조인해서, WHERE절로 2021년 가입자 필터를 건다. 그 다음 YEAR, MONTH로 그룹바이 걸고 USER_ID 디스팅크트(중복제거)한다음 카운트쓰고, 년 월 별로 상품구매자의 개수 구한다. 그리고 1번에서 구한 전체 가입자 개수를 가져와서 비율을 구한다. 나머지는 오더바이로 마무리.