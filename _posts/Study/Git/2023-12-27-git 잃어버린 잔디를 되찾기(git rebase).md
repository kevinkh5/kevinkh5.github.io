---
title: "[Git] 잔디가 안심어진 이유와 잃어버린 잔디 되찾기 (git rebase)"
author: baduk
date: 2023-12-27 14:04:00 +0900
categories: [Study, Git]
tags: [Git]
---
## 왜 잔디가 안심어졌을까?
일단 커맨드창에서 아래 명령어를 통해 config에 등록된 이메일을 확인해 본다.

```shell
git config user.email "이메일 주소"
```
만약 자신의 깃헙계정의 이메일과 다르다면, 커밋 푸쉬를 해도 본인계정에 잔디가 심어지지 않을 것이다. 따라서 아래 명령어를 통해서 config에 자신의 이메일을 등록한다. 깃헙 계정의 이메일은 프로필 사진을 클릭하여 Access - Emails에서 확인할 수 있다.

```shell
git config --global user.email "내 이메일 주소"
```

본인 이메일을 등록하였다면 앞으로 커밋 푸쉬를 했을 때 잔디는 잘 심어지게 될 것이다.

## 이미 못심어졌던 잔디는 어떻게 복구하는가?
일단 잔디 복구 원리는 잔디가 안심어진 날짜를 찾고, 그 곳에서 본인계정의 이메일이 아닌 다른 이메일로 커밋된 기록을 매뉴얼로 본인계정의 이메일로 변경하는 것이다. 이렇게 하면 잔디가 되살아난다.

그렇다면 어떻게 커밋을 변경할 수 있을까? 일단은 아래 명령어로 본인 커밋 내역을 확인한다.
```shell
git log --pretty=format:"%h = %an , %ar : %s" --graph
```

그리고나서 본인이 변경해야하는 커밋의 해시를 찾는다. 여기서 본인이 변경해야하는 해시보다 더 오래된 해시를 복사한다.(주의해야 할 것은 본인이 변경하고자 하는 해시를 복사하는게 아니라 그 이전것을 복사해야한다)

그리고 아래 명령에서 해시코드 부분에 본인 해시코드를 넣고 명령을 실행한다.
```shell
git rebase -i -r 해시코드
```

실행하고 나면 Commands:~~~~라는 문구와 함께 창이 뜨는데, 여기서 본인이 이메일을 변경해야하는 커밋의 해시 옆에 pick을 edit으로 변경한다. 여기서 vi편집을 해야하는데 a버튼을 누르면 글자를 insert할 수 있다. edit으로 변경했으면 esc한번 누르고 `:`(콜론)입력하고 `wq!`한 후에 저장 후 나간다.

그리고 이제는 해당 edit 하는 커밋에서 본인이 변경하고자 하는 이메일을 입력해주면된다. 아래 명령을 실행하자.

```shell
git commit --amend --author="이름<이메일>"
```
위 명령을 실행하면 Add README창이 실행되는데, 그냥 `esc`누르고 `:q`입력하고 나간다. 그러면 이렇게 한 커밋이 변경완료된 것이다.

여기서 또 주의해야할 점은 이 상태에서 바로 푸시를 할 경우, everything up to date라고 뜨면서 푸시가 안된다. 이 부분에서 많이 헤맸는데, 반드시 아래 명령을 실행하여 `Successfully rebased and updated ~~~ `이 부분을 결과로 받아야 한다.
```shell
git rebase --continue
```
만약 edit 한게 여러개 있다면 그 여러개에 대해서 모두 `git commit --amend --author="이름<이메일>"` 이 명령을 완료하면 `git rebase --continue`를 실행했을 때 `Successfully rebased and updated ~~~ `결과를 받을 수 있을 것이다.

`Successfully rebased and updated ~~~ `결과를 받았으면 아래 명령을 통해서 강제푸시를 해준다.

```shell
git push origin +main
```
main앞에 + 붙인것은 강제를 의미한다.

이렇게하고 본인 계정의 깃헙페이지에 들어가보면 잔디가 복구된 것을 확인할 수 있다.

