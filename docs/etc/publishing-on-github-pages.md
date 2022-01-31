---
layout: default
title: GitHub Pages로 배포하기
last_modified_date: 2021-02-14 16:02:44
parent: Etc
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">GitHub Pages로 배포하기</div>

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

GitHub의 [GitHub Pages](https://pages.github.com/)를 이용하면 `.github.io` 라는 url의 웹 페이지를 무료로 호스팅할 수 있다. 그래서 보통은 개인 블로그와 같은 정적 페이지를 배포할 때 많이 사용하지만, 나는 이번에 간단한 프로젝트의 웹 사이트를 만들기 위해 사용했다. (참고로 지금 글을 적고 있는 현재 블로그는 Netlify를 이용해 배포한 사이트이다.)

# GitHub Pages로 배포 과정

## 1. GitHub Repository 만들기

[GitHub](https://github.com/)에서 배포하고자 하는 소스들을 저장할 repository를 만들어야 한다. 이미 repository가 있다면 기존 repository를 사용하면 된다. 이때 repository는 반드시 public 저장소여야 한다.

![gh-pages1](/assets/images/etc/gh-pages1.png)

## 2. gh-pages 브랜치 만들기

다음으로 `gh-pages`라는 이름의 branch를 만들어야 한다. 배포하고자 하는 소스 코드가 해당 branch에 있다는 것을 알려주기 위해서는 반드시 gh-pages라는 이름으로 만들어야 한다.

![gh-pages2](/assets/images/etc/gh-pages2.png)

만약 sourcetree나 github desktop과 같은 프로그램을 이용해 branch를 생성한 경우에는 push 하는 것을 잊지 말자. local 환경에 만든 branch를 remote 환경인 GitHub에도 push를 해야 GitHub에서 해당 branch를 확인할 수 있다.

## 3. Environments의 github-pages를 클릭

![gh-pages3](/assets/images/etc/gh-pages3.png)

## 4. Deployments의 View deployment 버튼 클릭

Deployments의 View deployment 버튼을 누르면 `username.github.io/a name of the project(gh-pages)` 라는 url을 가진 웹 사이트로 이동할 수 있다.

![gh-pages4](/assets/images/etc/gh-pages4.png)

# gh-pages로 배포 후 업데이트

이미 배포를 한 후에 소스 코드를 수정할 일이 생길 수 있다. 매우 간단하게 업데이트를 할 수 있다.

1. 먼저 소스코드를 main(master) branch에서 수정한 후 commit & push를 한다.
2. 그 다음 gh-pages를 main(master)와 같도록 업데이트를 해준 뒤에 push를 하면 된다.
3. Deployments에서 새로고침을 하면 gh-pages가 새롭게 배포된 것을 확인할 수 있다. 하지만 무료 서비스이기 때문에 실시간으로 반영되지 않을 수 있으므로 3~5분 정도 기다려야할 수 있다.

   > 제일 위에 있는 것이 가장 최근에 배포된 것이다.

   ![gh-pages5](/assets/images/etc/gh-pages5.png)

### Github Desktop

Github Desktop을 이용하는 경우 main branch에서 수정한 뒤에 Current Branch를 gh-pages로 바꾸고 Branch 탭에서 Update from master 클릭하면 master 브랜치와 같은 상태가 된다. 업데이트 후에 push하면 수정한 내용이 적용돼서 새롭게 배포된다.

### Sourcetree

Sourcetree를 이용하는 경우 main branch에서 수정한 뒤에 gh-pages branch로 체크아웃한 다음 origin/main의 원격 저장소 브랜치 추적을 하면 main branch를 추적하게 되고 이 때 pull을 하면 수정한 내용이 gh-pages branch에 업데이트 된다. 업데이트 후에 push하면 수정한 내용이 적용돼서 새롭게 배포된다.

![gh-pages6](/assets/images/etc/gh-pages6.png)
