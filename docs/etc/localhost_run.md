---
layout: default
title: 다른 컴퓨터에서 localhost 연결하기
last_modified_date: 2021-06-07 23:06:97
parent: Etc
---

# 다른 컴퓨터에서 localhost 연결하기

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

Front와 Back을 나누면서 프로젝트를 진행하다보니 배포하기 전에는 서로가 개발한 것을 확인하지 못해 불편한 점들이 생겼다. 예를 들어, Front 입장에서는 Back에서 만든 REST API를 이용하지 못했고, Back 입장에서는 결과물으로 시각적으로 확인하기 어렵다는 점이다.

이 때 [localhost.run](https://localhost.run/) 이라는 사이트를 이용하면 내 localhost를 다른 컴퓨터에서, 또는 다른 컴퓨터의 localhost를 내 컴퓨터에서 확인할 수 있다.

# localhost.run

- `[localhost.run](http://localhost.run)` 은 인터넷 엑세스 가능 URL에서 로컬로 실행중인 애플리케이션을 즉시 사용할 수 있도록 하는 client-less 도구이다
- 모든 주요 운영 체제에는 이미 SSH가 설치되어 있고, localhost.run은 SSH를 클라이언트로 사용하므로 서비스를 사용하기 위해 따로 다운로드할 필요가 없으며 무료 도메인에 대한 계정 설정도 필요하지 않아 매우 쉽게 사용할 수 있다.
- localhost.run은 무료이지만, 그렇기에 몇 가지 제한 사항이 있다.
  1. 도메인 이름은 몇 시간 후에 변경된다
  2. 속도 제한이 있다

## 내 localhost를 다른 컴퓨터에서 연결하기

### 1. 아래 명령어 실행

내 localhost를 다른 컴퓨터에서 연결하기 위해서는 [localhost.run](https://localhost.run/) 사이트에 접속하면 확인할 수 있는 명령어를 이용해 터미널에 입력하면 된다. 단, 포트 번호는 내가 공유하고자 하는 포트 번호를 넣어주면 된다.

![localhost](/assets/images/etc/localhost.png)

> 포트 3000번에서 로컬로 실행되는 (리액트) 어플리케이션에 인터넷 도메인 연결하기

```jsx
ssh -R 80:localhost:3000 nokey@localhost.run
```

### 2. 명령어를 실행한 후 제공되는 URL 공유

명령어를 실행하게 되면 맨 마지막 줄에 `https://주소.localhost.run` 이라는 URL을 확인할 수 있다. 해당 URL을 내 localhost에 접근하고 싶은 다른 사람들에게 공유하면 이 URL을 통해서 내 localhost를 확인할 수 있다. 단, 일단 URL을 공유하기 전에 내가 먼저 내 localhost를 실행해줘야 한다!

### ⚠️ URL 주소는 달라진다

무료로 이용하고 있기 때문에 URL 주소는 내가 명령어를 실행할때마다 달라지게 된다. 주로 개발 단계에서 서로 맞춰볼 때만 사용할 계획이라 크게 상관없지만, 혹시라도 달라지는게 싫다면 아래 링크 참고하기!

[FAQ - localhost.run](https://localhost.run/docs/faq#my-tunnel-name-changes-every-time-i-connect)

## 다른 컴퓨터의 localhost를 내 컴퓨터에서 연결하기

사실 이 과정은 너무 쉽다... 앞에 얘기했던 `내 localhost를 다른 컴퓨터에서 연결하기` 이 과정을 상대방이 진행한 다음 URL을 나에게 공유해주면 되기 때문이다. 따라서 내가 REST API를 이용해서 백엔드로부터 데이터를 GET으로 받아오거나 POST로 전송한다면 공유해준 URL을 이용하면 된다.

# Ref

- [localhost.run - localhost.run](https://localhost.run/)
