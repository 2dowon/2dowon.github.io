---
layout: default
title: Git Stash 사용법 - 커밋하지 않고 변경사항 임시저장 하기
last_modified_date: 2021-07-11 18:07:36
parent: Etc
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Git Stash 사용법 - 커밋하지 않고 변경사항 임시저장 하기</div>

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

프로젝트를 진행하다보면 브랜치를 이용해 기능을 개발하게 된다. 이때 다른 브랜치로 이동(체크아웃)하기 위해서는 현재 브랜치에서 작업하던 내용들을 다 커밋을 해야 한다. 그렇지 않으면 그동안 작업하던 내용들이 다 날라갈 수 있다. 나는 같은 이유로 몇번 작업하던 코드들을 날렸던 적이 있어서 불안한 마음에 로컬 환경에 작업하던 코드 파일을 복사했던 적도 있었다...ㅎㅎ

그렇지만 사실 개인 프로젝트를 할 때는 브랜치마다 옮겨다닐 일이 많지 않아서 따로 방법을 찾아보지 않다가, 이번에 좀 불편해져서 알게 된 기능이 바로 `Git Stash` 이다.

# Git Stash

- 변경사항을 임시로 저장할 수 있도록 도와주는 기능
- Git 저장소에 관리하고 있는 파일들을 대상으로 실행된다. 따라서 한 번도 커밋된적 없는 파일은 아직 추적되지 않기 때문에 임시로 저장할 수 없다. (이 파일들의 경우, 따로 저장하지 않고 다른 브랜치로 체크아웃해도 문제가 생기지 않는다.)

## Git Stash 명령어

### 임시 저장

```bash
git stash
```

### 임시 저장된 변경사항을 다시 가져오기

```bash
git stash pop
```

## 소스트리를 활용한 Git Stash

위 명령어들을 터미널에서 직접 입력해도 되지만, 소스트리와 같은 GUI 툴을 사용하면 훨씬 쉽게 Git Stash를 이용할 수 있다.

### 임시 저장

![stash1](/assets/images/etc/stash1.png)

- 소스트리를 이용해 스태시 기능을 사용하기 위해서는 커밋, 풀 등의 버튼들 옆에 있는 `스태시` 버튼을 이용하면 된다.
- 스태시 버튼을 누르면 모달창이 하나 뜨고 다시 스태시 버튼을 클릭하면 임시 저장이 된다. 이때 메세지의 경우 선택 사항이므로 입력하지 않아도 된다.

### 임시 저장된 변경사항을 다시 가져오기

![stash2](/assets/images/etc/stash2.png)

- 워크스페이스, 브랜치, 태그 등을 확인할 수 있는 공간의 아래쪽에 보면 `치워두기` 탭이 있다.
- `치워두기` 탭에서 밑에 있는 `WIP ~~` 가 아까 임시 저장한 변경사항들이다. 아까 메세지를 빈칸으로 저장하게 되면 `WIP` 이름으로 시작되고, 메세지를 남길 경우 그 이름으로 저장된다.
- 더블 클릭하면 임시 저장한 변경사항들을 다시 가져와서 적용할 수 있다.
- 적용한 변경사항들은 삭제해둬야 나중에 헷갈리지 않는다.

# Ref.

- [[2019.09.13] git stash 사용법 - 현재 상태를 저장해보자](https://helloinyong.tistory.com/202)

- [git stash 사용법: 커밋하지 않고 변경사항 저장하는 방법](https://www.lainyzine.com/ko/article/git-stash-usage-saving-changes-without-commit/)
