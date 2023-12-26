---
title: 깃헙페이지 업로드 에러 chirpy theme, ‘a’ tag is missing a reference
author: baduk
date: 2023-12-12 12:39:00 +0900
categories: [Problem solving, 에러일기]
tags: [problem, error]
---
## 문제
깃헙 블로그에서 새로운 게시글을 작성해서 업로드 하는데 에러가 발생하였다.

```
  'a' tag is missing a reference
```

## 원인
원인을 확인해본 결과, _data/authors.yml 해당 파일 안에, 새로운 글을 post할 때 작성한 author에 대한 정보가 없었고, build 과정에서 _site/posts/newpost1/index.html 파일에 href가 비어있게 되면서 에러가 발생한 듯함 


## 해결
따라서 _data/authors.yml 해당 파일 안에, author에 대한 정보를 추가한 후(나는 baduk이라는 author에 대한 정보를 추가함) 깃 푸쉬하여 해결하였음. author에 대한 정보를 추가하는 방법은 authors.yml위에 주석으로 어떻게 추가해야하는지 작성되어있음.



### 참고:
<https://talk.jekyllrb.com/t/chirpy-theme-a-tag-is-missing-a-reference/8731>