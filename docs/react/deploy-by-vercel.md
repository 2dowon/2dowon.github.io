---
layout: default
title: React + Next.js - Vercel로 배포하기
date: 2021-11-16 12:11:53
parent: React
nav_order: 4
---

# React + Next.js - Vercel로 배포하기

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

Next.js 프레임워크를 이용해 React 프로젝트를 만들었다면 Vercel로 배포할 수 있다. 참고로 CRA를 이용해서 React 프로젝트를 진행할 때는 GitHub Pages나 Netlify를 이용해서 배포했었다. (GitHub Pages 배포 방법은 이전에도 정리해놓은 글이 있어서 혹시라도 궁금하다면 [이 글](https://2dowon.netlify.app/etc/publishing-on-github-pages/)을 참고해주세요!)

✅ Next.js 프레임워크를 이용해 React 프로젝트를 만들었음을 전제로 합니다.

# Vercel

- Vercel은 Next.js 개발 팀에서 만든 호스팅 사이트이다.
- SSR을 사용한 프로젝트를 배포할 수 있다.
  (참고로 앞서 말했던 배포 방법 중 GitHub Pages는 정적 웹 페이지 호스팅이기 때문에 SSR은 사용할 수 없다.)

# Vercel 배포하기

## 1. Vercel 로그인

![vercel1](/assets/images/react/vercel1.png)

[Vercel](https://vercel.com/) 사이트에서 로그인을 한다. GitHub, GitLab, Bitbucket 등을 이용해 로그인할 수 있는데, 본인이 배포하고자 하는 프로젝트의 소스코드가 있는 곳으로 로그인을 하면 된다. 나는 GitHub을 통해 소스코드를 관리하고 있기 때문에 GitHub으로 로그인을 했다.

## 2. New Project 생성

![vercel2](/assets/images/react/vercel2.png)

로그인을 했다면 대시보드로 넘어오게 되는데, 거기서 New Project를 생성할 수 있다.

## 3. Import Git Repository

![vercel3](/assets/images/react/vercel3.png)

배포하고자하는 프로젝트의 repo를 선택해서 import 한다.

## 4. Deploy

![vercel4](/assets/images/react/vercel4.png)

프로젝트를 import 했다면 이제 배포할 수 있다.

- `Create a Team`의 경우 개인 프로젝트를 배포하는 상황이므로 나는 skip했다.
- `Configure Project`에서 Vercel에서 사용할 프로젝트 이름과 `Framework`가 Next.js로 잘 설정되었는지 확인한다.
- 프로젝트의 `Root Directory`도 설정할 수 있는데, 보통의 경우에는 `./` 이므로 비워도 된다. 만약 import한 repo안에 특정 폴더 안에 있는 프로젝트를 배포한다면 상황에 맞게 해당 경로를 수정하면 된다.
- `Build and Output Settings`의 경우, 본인 프로젝트의 package.json에서 확인했을 때 build 명령어가 next build로 되어있다면 굳이 따로 바꿀 필요없다. 즉, 기존 프로젝트에서 build 명령어를 바꾼 적이 없다면 그대로 냅두면 된다. 바꾼적이 있다면 바꾼대로 수정하기!
- 만약 프로젝트에서 `.env.local` 파일을 통해 환경변수를 사용하고 있다면 `Envrionment Variables` 여기서 환경변수를 추가해줘야한다. 나는 처음에 이 부분에 환경변수를 추가하지 않아서 빌드가 실패했었다..😥 물론 까먹고 여기서 추가하지 못했다면 나중에 따로 setting에서 추가할 수 있는데, 그건 아래서 확인!
  (보통 환경변수는 `.env` 파일을 이용하지만, Next.js에서는 `.env.local` 파일을 생성한다. 그리고 환경변수에 설정하는 prefix는 REACT_APP 이 아니라 NEXT_PUBLIC을 사용한다.)

## 5. 배포가 잘 되었는지 확인 + 배포 주소

![vercel5](/assets/images/react/vercel5.png)

배포가 잘되었다면 대시보드에서 배포한 프로젝트를 클릭했을 때 위처럼 보일 것이다. Domains에서 배포된 주소를 확인할 수 있다. 도메인은 아래 사진처럼 총 3개정도 생성되는 것 같다.

![vercel6](/assets/images/react/vercel6.png)

## ✅ 환경 변수 설정하기

만약 배포할 때 환경 변수를 설정하는 것을 까먹었다면 프로젝트의 Settings에서 설정할 수 있다.

![vercel7](/assets/images/react/vercel7.png)

Project Settings - Environment Variables 탭에 들어오면 Add New를 통해서 환경변수를 추가할 수 있다. 환경변수를 추가하면 이미지 맨 아래에 있는 것처럼 확인할 수 있다. 저기서 NEXT_PUBLIC_AIRTABLE_KEY가 내가 추가해준 환경변수이다.

환경변수를 추가한 후 다시 재배포를 하면 환경변수가 적용되어 배포된 것을 확인할 수 있다.

![vercel8](/assets/images/react/vercel8.png)

재배포는 Deployments 탭에서 할 수 있다. 만약 환경변수를 제대로 설정하지 않았다면 저기 맨 마지막 배포처럼 Error로 되어있을 것이다. Error가 난 배포를 Redeploy로 재배포해주면 된다.

# Ref.

- [[next.js] next.js, react, vercel로 30분만에 웹사이트 배포하기](https://velog.io/@gwsyl22/next.js-next.js-react-vercel%EB%A1%9C-30%EB%B6%84%EB%A7%8C%EC%97%90-%EC%9B%B9%EC%82%AC%EC%9D%B4%ED%8A%B8-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0)

- [NextJS에서 환경변수 다루기(with vercel)](https://velog.io/@gytlr01/NextJS%EC%97%90%EC%84%9C-%ED%99%98%EA%B2%BD%EB%B3%80%EC%88%98-%EB%8B%A4%EB%A3%A8%EA%B8%B0with-vercel)

- [[Next] Next.js 에서 환경변수 사용해보기 (feat. Vercel)](https://tigger.dev/entry/Nextjs-%EC%97%90%EC%84%9C-%ED%99%98%EA%B2%BD%EB%B3%80%EC%88%98-%EC%82%AC%EC%9A%A9%ED%95%B4%EB%B3%B4%EA%B8%B0-feat-Vercel)
