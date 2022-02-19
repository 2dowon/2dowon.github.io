---
layout: default
title: npm peerDependencies error
last_modified_date: 2021-05-07 11:05:21
parent: Etc
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">npm peerDependencies error</div>

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

# npm peerDependencies error

리액트 18버전에서 상태관리 라이브러리 중 하나인 `jotai` 를 설치하려고 했으나 아래와 같은 에러가 발생했다. 에러를 읽어보면 **dependency conflict** 로 인한 에러이고, 해결책으로 `--force` 또는 `--legacy-peer-deps` 를 제시하고 있다. `--force` 옵션을 이용해 에러를 해결했으나, 이 에러가 왜 발생했으며 두 해결방안에는 어떤 차이가 있는지 아래에서 간단히 정리해두려고 한다.

![npm-peerDependencies-error1](/assets/images/etc/npm-peerDependencies-error1.png)

## `peerDependencies`

실제로 패키지에서 `require`나 `import`하지는 않지만, 특정 라이브러리나 툴에 호환성을 필요로 할 경우에 명시하는 dependencies이다. npm3 부터 6까지는 `peerDependencies`가 자동으로 설치되지 않았고, 만약 버전이 맞지 않는 경우 에러가 아닌 warning(경고 문구)만 보였다. 하지만 npm@7 부터는 `peerDependencies`가 기본으로 설치되고, 이 버전이 맞지 않으면 에러가 발생한다.

> package.json

```json
{
  "name": "jotai-todolist",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "next": "^12.0.10",
    "react": "^18.0.0-rc.0",
    "react-dom": "^18.0.0-rc.0"
  },
  "devDependencies": {
    "@types/node": "17.0.18",
    "@types/react": "17.0.39",
    "autoprefixer": "^10.4.2",
    "eslint": "8.9.0",
    "eslint-config-next": "12.0.10",
    "postcss": "^8.4.6",
    "prettier": "^2.5.1",
    "prettier-plugin-tailwindcss": "^0.1.7",
    "tailwindcss": "^3.0.22",
    "typescript": "4.5.5"
  }
}
```

위 `package.json` 을 살펴보자. react 버전은 ^18.0.0-rc.0이고, jotai를 설치하려고 시도했다. 그러나 jotai는 `peerDependencies`로 react@>=16.8를 요구하기 때문에 **dependency conflict**가 발생해 npm@7 환경에서는 설치가 되지 않는다. (npm6까지는 경고문구만 발생하고, 설치는 된다!)

## npm peerDependencies error 해결하기

- \***\*`--force` :** package-lock.json에 몇가지의 다른 의존 버전들을 추가함으로써 conflict가 통과
  > 나는 아래처럼 `--force` 를 이용해서 설치했다
  > ![npm-peerDependencies-error2](/assets/images/etc/npm-peerDependencies-error2.png)
- \***\*`--legacy-peer-deps`\*\*** : peerDependency가 맞지 않아서 conflict에 대한 warning을 발생시키지만 일단 설치
- \***\*`--strict-peer-deps`** : peer dependency를 strict하게 검사

# 🍪 npm install option

- `**-g**` : 글로벌로 설치를 진행
- `**--dry-run**` : 아무것도 설치를 진행하지 않지만 설치하는 것과 똑같이 로그가 남는다

### npm install option을 통해 dependency에 어떻게 저장할 것인지 정하기

- **`-P`** or **`--save`** : package.json의 dependencies에 패키지를 등록 ⇒ **defalut**
- **`-D`** or **`--save-dev`** : package.json의 devDepndencies에 패키지를 등록
- **`-O`** or **`--save-optional`** : package.json의 optionalDependencies에 패키지를 등록
- **`--no-save`** : dependencies에 패키지를 등록 X

✅  예전에는 `--save` 옵션을 통해서 package.json의 dependencies에 패키지를 등록하도록 했는데 npm5 이후부터는 default 값으로 적용되기 때문에 더 이상 쓰지 않아도 된다.

# Ref.

- [package.json에 쌓여있는 개발 부채](https://yceffort.kr/2021/10/debt-of-package-json)

- [npm install `--force` and `--legacy-peer-deps` 차이점](https://velog.io/@yonyas/Fix-the-upstream-dependency-conflict-installing-NPM-packages-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0%EA%B8%B0)

- [npm: When to use `--force` and `--legacy-peer-deps`](https://stackoverflow.com/questions/66020820/npm-when-to-use-force-and-legacy-peer-deps)

- [[NPM] npm install 할 때 --save 옵션을 함께 입력하는 이유? 하지만 이제는 사용하지 않아도 되는 이유.](https://xtring-dev.tistory.com/11)
