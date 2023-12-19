---
title: '[Linux] Command Structure'
author: baduk
date: 2023-12-13 18:47:00 +0900
categories: [Study, Linux]
tags: [Linux]
---

```bash
ping -c 1 127.0.0.1
```

위 커맨드는 인터넷연결이 잘 되어있는지 확인하는 코드인데, 한번 분석해보자.

`ping` -> command's name

`c` -> command's options

`1` -> options value

`127.0.0.1` -> command argument

***

```bash
ls
```
위와 같이 option이나 argument가 없어도 error가 발생하지않는 command도 있다.

***
```bash
df
```
df command displays information about the filesystem and the disk space usage

