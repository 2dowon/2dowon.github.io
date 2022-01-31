---
layout: default
title: Sourcetree를 이용해 Github과 Gitlab 연동하기
last_modified_date: 2021-10-22 00:10:68
parent: Etc
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Sourcetree를 이용해 Github과 Gitlab 연동하기</div>

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

팀 프로젝트를 하면서 평소에는 Github을 사용해서 소스코드를 관리하고 있다. 하지만 소마에서는 팀 프로젝트 코드 제출을 Gitlab으로 해야했다. 이 점 때문에 굳이 익숙하지 않은 Gitlab을 사용하고 싶지는 않았고, 그렇다고 제출 직전에 소스코드를 한번에 업로드하기에는 그동안 꾸준히 진행해왔던 커밋들이 아까웠다.

그래서 Github과 Gitlab 연동하는 법에 대해서 찾아봤고, 여러 방법 중에서도 현재 사용하고 있는 Sourcetree(소스트리)를 통해 쉽게 연동하는 법에 대해 정리하려고 한다.

## 전제조건

- Github에서 이미 연동하고자 하는 프로젝트의 소스코드를 관리하고 있음을 전제로 합니다.
- Sourcetree(소스트리)를 이용하고 있음을 전제로 합니다.

## Github과 Gitlab 연동하기

### 1. Gitlab에서 새로운 프로젝트 생성하기

Github에는 이미 프로젝트 소스코드를 관리하고 있는 레포지토리가 있다는 전제 하에, Gitlab에만 새로운 프로젝트를 생성한다. 이미 Github에 프로젝트가 있기 때문에 import 해야하지 않을까라는 생각이 들 수 있지만, 우리는 빈 프로젝트에 소스코드를 새롭게 푸시할 것이므로 빈 프로젝트를 생성하면 된다.

![gitlab1](/assets/images/etc/gitlab1.png)

### 2. Sourcetree에서 원격 저장소 경로 추가하기

Sourcetree(소스트리)를 통해 관리하고 있는 저장소 중에서 연동하고자 하는 프로젝트와 관련된 저장소를 들어가서 설정을 선택한다. 설정에서 원격을 선택하면 아마 `origin`이라는 이름으로 `git@github.com:프로젝트 주소` 의 경로가 하나 있을 것이다. 그러면 이제 원격 저장소 경로를 gitlab 주소로 추가하면 된다. 원하는 이름을 설정하고, 경로는 아까 프로젝트를 만들었기 때문에 해당 프로젝트의 url 주소를 입력하면 된다.

![gitlab2](/assets/images/etc/gitlab2.png)

### 3. Github 푸시할 때 Gitlab에도 푸시하기

Sourcetree(소스트리)를 이용해서 푸시를 하면 다음과 같은 창이 익숙할 것이다. Github에만 푸시할 때는 `origin` 저장소에만 푸시했지만, 이제 우리는 Gitlab에도 푸시하고 싶으므로 아까 추가했던 이름과 경로의 저장소(사진에서는 `orgin_`)를 선택하고, 푸시할 브랜치도 선택해서 확인을 누르면 푸시할 수 있다.

![gitlab3](/assets/images/etc/gitlab3.png)

이 과정을 통하면 Gitlab에도 기존에 Github에 있던 소스코드를 연동할 수 있고, Github에 존재하던 커밋 로그 역시 그대로 Gitlab으로 가져올 수 있다.
