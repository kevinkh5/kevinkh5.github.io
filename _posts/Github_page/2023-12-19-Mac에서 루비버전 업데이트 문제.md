---
title: Mac에서 루비버전 업데이트 문제로 github page 로컬서버 가동 못할때 해결법
author: baduk
date: 2023-12-19 20:50:00 +0900
categories: [Github page, Error]
tags: [problem, error]
---
Mac에서 루비 버전업데이트가 안되어서 github page로컬 서버 가동을 할 수 없었다.

하지만 아래 커맨드로 루비 버전업데이트를 성공 시켰고, 정상적으로 로컬서버를 가동시킬 수 있었다.

```shell
$ asdf plugin add ruby
```
먼저 위와같이 루비 asdf plugin을 설치한다

```shell
$ asdf list all ruby
```
그리고 사용가능한 루비 버전을 확인한다

```shell
$ asdf plugin-update ruby
$ asdf list all ruby
```
만약 사용가능한 루비 버전이 보이지 않는다면, 플러그인 업데이트를 한 후 다시 시도해본다

```shell
$ asdf install ruby latest
```
루비 최신버전을 설치한다. 이 과정에서 5분정도 소요된다

```shell
$ asdf global ruby 3.3.0
```
루비 기본버전을 설정해주기 위해 위와 같이 커맨드를 입력한다. `3.3.0`부분은 본인이 설치한 버전에 맞게 수정해준다.

설정이 끝났으면 터미널창을 껐다가 다시 켜준다.

---
위 과정이 끝나고 다시 터미널창에 `ruby -v`를 입력하면, 3.0이상의 루비 최신버전이 설치되어있는것을 확인할 수 있다. 그리고 다시 터미널창에서 github page폴더가 있는곳으로 경로를 설정해준후

```shell
$ bundle
```
위 커맨드를 입력하여 번들을 설치하고

```shell
$ jekyll serve
```
위 커맨드를 통해서 로컬서버를 가동시켜주면 끝이난다.


맥에서 루비버전업데이트가 안돼서 며칠 헤매다가 포기하고 있었고, 며칠만에 다시 시도했는데, 너무 쉽게 해결되었다.👍

위 내용 출처: <https://mac.install.guide/ruby/6.html>