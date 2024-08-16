---
title: '[Python] 설치한 라이브러리 버전 requirements파일에 담는 명령과 설치하는 명령'
author: baduk
date: 2024-08-12 07:24:00 +0900
categories: [Programming Language, Python]
tags: [Python, 파이썬]
---

 아래 명령을 통해 설치된 패키지와 그 버전 정보를 쉽게 추출하여 requirements.txt 파일로 저장할 수 있다.
 
```shell
pip freeze > requirements.txt
```

아래 명령을 통해 작성된 requirements.txt를 불러와 pip를 설치할 수 있따.
```shell
pip install -r requirements.txt
```