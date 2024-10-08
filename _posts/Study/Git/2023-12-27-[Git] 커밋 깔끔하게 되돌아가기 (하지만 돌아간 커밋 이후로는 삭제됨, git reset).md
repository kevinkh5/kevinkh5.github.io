---
title: "[Git] 커밋 깔끔하게 되돌아가기 (하지만 돌아간 커밋 이후로는 삭제됨, git reset)"
author: baduk
date: 2023-12-27 14:40:00 +0900
categories: [Study, Git]
tags: [Git]
---
## git reset
git reset 사용하면 이전 커밋으로 되돌릴 수 있다. 단, 주의할점은 내가 돌아간 시점 이후 모든 커밋은 다 지워진다. 이건 협업시 사용하는 것은 위험할 듯하고 개인 프로젝트에서만 써야할 듯 싶다. 


## 사용법
사용은 간단하다. 먼저 아래 명령을 통해서 돌아가고자 하는 커밋의 해시를 찾는다.
```shell
git log
```

그리고 아래 두 명령중 하나를 사용하여 돌아가고자 하는 해시를 넣고 명령어를 실행하면된다.

```shell
git reset “돌아가려는 커밋 해시” 
```

또는

```shell
git reset --hard <커밋해시>
```

그런 다음 아래 명령어로 강제 푸시를 해주면 홈페이지에서 커밋이 이전으로 되돌아간 것을 볼 수 있고, 그리고 그 이전 이후로 커밋이 다 없어져 있는것을 확인할 수 있다.

```shell
git push origin +main
```

## 추가 작업
깃헙 홈페이지 레포지토리 상에서는 이전 커밋으로 되돌아가기는 했으나 로컬상태에서의 파일과 깃헙저장소에서의 파일이 동일하지 않을 수 있다. 싱크를 맞춰야한다.

로컬과 저장소에 있는 파일이 동일하게 유지되도록 하기 위해서 아래 명령어를 실행한다

```shell
git reset --hard origin/main
```
위 명령을 실행하면 없던 파일이 생기지만, 레포지토리에는 없고 로컬상에는 있는 파일이 없어지지는 않는것 같다? 이 부분은 추후 다시 확인해봐야겠다.


