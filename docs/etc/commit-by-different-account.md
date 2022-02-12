---
layout: default
title: GIT - 프로젝트마다 다른 계정으로 커밋하기
last_modified_date: 2022-2-12 22:31:28
parent: Etc
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">GIT - 프로젝트마다 다른 계정으로 커밋하기</div>

개인적으로 사용하는 git 계정과 회사에서 사용하는 git 계정이 다르기 때문에 특정 git 계정을 이용해서 commit, push해야하는 일이 생겼다. 처음에 아무 생각없이 commit, push했다가 개인 계정으로 회사 레포에 커밋을 하거나 그런 경우가 생겼다...😥

이처럼 git 계정이 여러개 있는 경우, 프로젝트마다 다른 git 계정으로 설정해야 하는 경우가 생긴다.

보통의 경우, git 설치 이후 아래처럼 global하게 본인의 git 계정을 설정해둔다. 이렇게 설정을 해야 commit, push할 때마다 계정 정보를 입력하지 않아도 되기 때문이다.

```bash
git config --global user.name "이름"
git config --global user.email "이메일"
```

하지만 위에서 설정한 계정과 다른 계정으로 commit과 push를 하고 싶다면 위처럼 global한 정보를 바꿔도 되지만, 그 경우 앞으로는 다시 바뀐 정보로 commit, push가 될 것이다. 만약 본인의 git 계정을 아예 바꾼 경우라면 이렇게 해도 된다.

하지만 **현재 특정 project나 repository에서만 다른 계정으로 commit, push** 하고 싶은 경우라면 아래와 같이 `--local` 이용해서 바꿀 수 있다.

```bash
git config --local user.name "현재 레포에서 사용할 이름"
git config --local user.email "현재 레포에서 사용할 이메일"
```

(+) git 계정의 설정을 확인하고 싶다면 아래 명령어로 할 수 있다.

```bash
git config --list
```

# Ref.

- [깃(git) - 프로젝트/저장소마다 다른 계정 정보 사용하기](https://awesometic.tistory.com/128)
