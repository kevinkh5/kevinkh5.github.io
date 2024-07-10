---
title: "[FastAPI] uvicorn 서버 가상환경경로로 제대로 맞춰서 동작 시키기"
author: baduk
date: 2024-03-17 16:31:00 +0900
categories: [Framework, FastAPI]
tags: [Fast API]
---
## 문제상황
유비콘으로 FastAPI서버 가동할 때, 계속 파이썬 3.9버전으로 맞춰서 실행하는 이슈가 발생했다.

필요한 라이브러리를 분명 설치했는데, 그게 파이썬 프로젝트에서 생성한 파이썬 3.11에는 설치가 되었지만, 유비콘은 계속 파이썬 3.9를 실행시켰고, 그 라이브러리를 3.9에서는 설치시킬 수 없어서, 유비콘이 서버가동할 때 사용하는 파이썬 경로를 파이썬 3.11로 가리키도록 해야 했다.

## 해결
터미널에 아래와 같이 커맨드를 작성하여, uvicorn에서 실행할 python경로를 잡아 문제를 해결했다.

```shell
~/PycharmProjects/fastapi_project/venv/bin/python3.11 -m uvicorn backend:app --reload
```