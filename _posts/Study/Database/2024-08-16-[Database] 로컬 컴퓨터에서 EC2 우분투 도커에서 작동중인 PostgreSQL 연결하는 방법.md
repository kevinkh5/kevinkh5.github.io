---
title: "[Database] 로컬 컴퓨터에서 EC2 우분투 도커에서 작동중인 PostgreSQL 연결하는 방법"
author: baduk
date: 2024-08-16 07:45:00 +0900
categories: [Study, Database]
tags: [Database]
---
## 1. AWS-EC2에 인바운드 규칙을 추가한다.
- 인바운드 규칙에 연결시도하는 컴퓨터 네트워크의 IP주소와 포트번호를 추가한다(ex.PostgreSQL은 보통 5432)

## 2. pg_hba.conf 파일을 찾아서 수정한다.
	
- PostgresSQL 컨테이너 안으로 들어가서 pg_hba.conf 파일 찾는다.
	- 리눅스 명령어 find를 사용하면 쉽게 찾을 수 있다. 
- vi편집기를 이용해 pg_hba.conf 파일내에 아래 규칙을 추가하고 저장한다.
```shell
host    all             all             (연결할 컴퓨터 네트워크의 IP)/32           md5
```

## 3. DB서버가 수정된 파일을 반영할 수 있도록, 아래 명령으로 도커 컨테이너를 내렸다가 다시 올린다.

- 정지 & 삭제
```shell
docker stop <container_name_or_id>
docker rm <container_name_or_id>
```

- 다시 띄우기
```shell
docker run --name <container_name> -v <host_volume>:/var/lib/postgresql/data -d postgres
```

- 만약 도커 컴포즈로 다시 올리고싶다면 아래 명령을 사용한다.
```shell
docker-compose up -d --build <service_name>
```


## 4. 이제 로컬 컴퓨터에서 pgadmin에 들어가서 서버를 추가한다.
1. General - Name을 적절히 적어준다.
2. Connection - Host name/address 본인 EC2 Public IP를 적어준다.
3. Connection - Port는 5432라면 5432 그대로 둔다.
4. Connection - Password - 본인 DB 비밀번호 입력하고 Save하면 연결 끝.