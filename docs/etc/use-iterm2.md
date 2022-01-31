---
layout: default
title: iTerm2 세팅하기
last_modified_date: 2021-06-26 15:06:64
parent: Etc
---

# iTerm2 세팅하기

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

Mac OS를 사용하고 있기 때문에 기존에는 터미널을 사용하고 있었지만, iTerm이 더 편하다는 말을 많이 들어서 몇달전부터는 iTerm2를 사용하고 있었다. 처음에는 이것저것 찾아보면서 우당탕탕 적용하느라 기록할 생각을 못했었는데, 이번에 Mac mini를 데려오면서 새로 세팅할 일이 생겨서 한번 정리해둘까 한다!

# homebrew 설치

맥을 사용하고 있다면 당연히 [homebrew](https://brew.sh/index_ko) 가 설치되어 있을 가능성이 높다. 만약 설치가 안되어 있다면 일단 터미널에서 아래 명령어를 이용해 설치할 수 있다.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

> homebrew를 설치했다면 반드시 아래 명령어로 제대로 설치되었는지 확인

```bash
brew help
```

아래처럼 보인다면 제대로 설치된 것!

![iTerm_1](/assets/images/etc/iTerm_1.png)

만약 `brew help` 명령어를 입력했을 때 위처럼 보이지 않는다면 이 [링크](https://velog.io/@dev_halo/M1-Mac-Home-brew%EB%A5%BC-%EC%84%A4%EC%B9%98%ED%95%B4%EB%B3%B4%EC%9E%90) 참고!

# zsh & oh-my-zsh 설치

### brew를 이용해 간단하게 `zsh` 설치

```bash
brew install zsh
```

### `zsh` 제대로 설치되었는지 확인

```bash
zsh --version
```

### `zsh` 를 기본 쉘로 바꾸기

```bash
which zsh # zsh 위치 확인
chsh -s `which zsh` # 현재 쉘을 zsh 쉘로 바꿈
```

### `oh-my-zsh` 설치

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

# iTerm2

## iTerm2 다운로드

[iTerm2](https://iterm2.com/) 홈페이지에서 다운로드 받을 수 있다

[iTerm2 - macOS Terminal Replacement](https://iterm2.com/)

## iTerm2 세팅하기

iTerm2의 Preferences에서 설정을 변경할 수 있다. Preferences 단축키는 `command` 와 `,` 를 동시에 누르면 된다.

> iTerm2 - Preferences

![iTerm_2](/assets/images/etc/iTerm_2.png)

### 폰트 설정

개인적으로는 `D2Coding` 폰트가 깔끔해서 이 폰트를 사용하고 있다. 이 [링크](https://github.com/naver/d2codingfont/releases/tag/VER1.3.2)에서 다운로드 받은 후 아래처럼 적용해주면 된다.

> Preferences - Profiles - Text - Font ⇒ D2Coding 선택

![iTerm_3](/assets/images/etc/iTerm_3.png)

### 테마 설정

zsh 쉘의 테마를 설정할 수 있는데, 보통 `agnoster` 를 많이 사용한다.

1. `.zshrc` 파일 수정

   - `vim ~/.zshrc` 명령어를 통해 수정하기
   - 직접 파일을 찾아서 수정하기

     ⇒ 보통 홈 디렉토리에 숨겨져 있다. (숨긴 파일을 볼 수 있는 단축키는 `shift` + `command` + `.` 이다)

2. `zsh_theme` 를 찾아서 수정한다 ⇒ `ZSH_THEME="agnoster"`

   ![iTerm_4](/assets/images/etc/iTerm_4.png)

   - vim으로 열었을 경우에는 `a` 를 눌러야 수정할 수 있고, 수정이 끝난 후에는 esc를 누르고 `:wq` 를 입력해서 저장한 후 종료할 수 있다.
   - 파일을 직접 찾아서 수정한 경우에는 평소처럼 저장하고 닫으면 된다

3. 마지막으로 수정사항을 반영해주기 위해서 아래 명령어를 실행한다 ⇒ `source ~/.zshrc`

### 터미널에 사용자 이름만 깔끔하게 설정하기

터미널의 사용자 이름만 깔끔하게 보여주기 위해서 `.zshrc` 파일의 맨 마지막에 아래 코드를 추가해준다.

![iTerm_bash1](/assets/images/etc/iTerm_bash1.png)

### New Line 적용하기

터미널의 명령어가 길어지다보면 화면을 벗어나기도 하고, new line으로 입력하게 되면 더 깔끔하게 보여서 아래처럼 보이도록 설정하는 것을 좋아한다.

![iTerm_5](/assets/images/etc/iTerm_5.png)

1. agnoster 테마를 설치했다는 기준으로 아래 명령어를 실행

   ```bash
   vi ~/.oh-my-zsh/themes/agnoster.zsh-theme
   ```

2. `build_prompt()` 를 찾아서 `prompt_newline` 을 추가해줘야 하는데 꼭 순서를 지켜서 추가해줘야 한다.

![iTerm_6](/assets/images/etc/iTerm_6.png)

3. 코드의 제일 밑으로 내려가서 아래처럼 코드를 입력

   ![iTerm_7](/assets/images/etc/iTerm_7.png)

   ![iTerm_bash2](/assets/images/etc/iTerm_bash2.png)

## oh-my-zsh 쉘 플러그인 설치

터미널 대신 iTerm2를 사용하는 이유가 사실 99%는 이 플러그인 때문이다.

### [autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

이 플러그인을 설치하면 내가 예전에 썼던 명령어를 기억했다가 추천해주기 때문에 매우 매우 편리하다!! 진짜 명령어를 입력하다보면 기억이 안나기도 하고, 오타도 많이 나는데 이 플러그인을 사용하면서 그 빈도가 매우 줄었다.

1. oh-my-zsh를 사용한다는 기준으로 아래 명령어를 실행

   ```bash
   git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
   ```

2. `~/.zshrc` 에 플러그인 경로 추가

   ```bash
   plugins=(
       # other plugins...
       zsh-autosuggestions
   )
   ```

   ![iTerm_8](/assets/images/etc/iTerm_8.png)

### [syntax highlighter](https://github.com/zsh-users/zsh-syntax-highlighting)

명령어가 유효하면 색깔을 바꿔주는 기능이다. 오타가 났을 경우에 빠르게 확인할 수 있고, 보기에도 좋아서 사용하는 편이다.

1. oh-my-zsh를 사용한다는 기준으로 아래 명령어를 실행

   ```bash
   git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
   ```

2. `~/.zshrc` 에 플러그인 경로 추가

   ```bash
   plugins=(
       # other plugins...
       zsh-syntax-highlighting
   )
   ```

   ![iTerm_9](/assets/images/etc/iTerm_9.png)

# Ref

- [iTerm2와 oh-my-zsh로 맥 터미널 꾸미기](https://medium.com/@taegeon/iterm2%EC%99%80-oh-my-zsh%EB%A1%9C-%EB%A7%A5-%ED%84%B0%EB%AF%B8%EB%84%90-%EA%BE%B8%EB%AF%B8%EA%B8%B0-80029f73be90)

- [ITerms, oh-my-zsh 테마 이용해 쉘 바꾸고, 유용한 플러그인 사용하기!](http://heetop.blogspot.com/2017/10/oh-my-zsh_12.html)

- [[개발 환경] iTerm2로 터미널 커스텀하기](https://ooeunz.tistory.com/21)
