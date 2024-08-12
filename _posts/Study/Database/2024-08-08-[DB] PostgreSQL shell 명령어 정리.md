---
title: "[Database] PostgreSQL shell 명령어 정리"
author: baduk
date: 2024-08-08 21:51:00 +0900
categories: [Study, Database]
tags: [Database]
---
## 데이터베이스 목록 확인
- \l

## 현재 접속중인 데이터베이스 확인
- \conninfo

## 테이블 목록 확인
- \dt

## 테이블 구조 보기
- \d < tablename >


## 테이블 추가
```sql
CREATE TABLE news_info( 
	id SERIAL PRIMARY KEY, 
	categoy VARCHAR(50), 
	title VARCHAR(100) NOT NULL
);
```
##  데이터 삽입
```sql
INSERT INTO news_info (categoy, title) VALUES ('AINEWS2’, 'NEWSCONTENT2’);
```