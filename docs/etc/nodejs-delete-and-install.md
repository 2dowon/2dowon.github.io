---
layout: default
title: node.js 재설치 및 node install 권한 오류(MAC)
last_modified_date: 2021-05-07 11:05:21
parent: Etc
---

# node.js 재설치 및 node install 권한 오류(MAC)

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

어느 순간부터 이상하게 `npm` 을 이용해 설치하는 것들이 제대로 설치는 되었지만, version을 확인하려고 하면 나오지 않는 문제가 발생했다. 처음에는 환경변수의 문제라고 생각하고 환경변수를 다시 설정하였으나 문제가 지속되었고, 결국 node.js를 재설치하기로 했다.

## (MAC) node 삭제

처음에 node를 삭제하기 위해서 아래와 같은 방법을 사용했는데, node가 제대로 삭제되지 않았는지 npm으로 설치한 것들의 문제가 여전히 발생했다.

> 🤔 이유는 모르겠지만, node 삭제가 제대로 되지 않았던 방법

```bash
sudo npm uninstall npm -g

sudo rm -rf /usr/local/lib/node /usr/local/lib/node_modules /var/db/receipts/org.nodejs.*
sudo rm -rf /usr/local/include/node /Users/$USER/.npm
sudo rm /usr/local/bin/node
sudo rm /usr/local/share/man/man1/node.1
sudo rm /usr/local/lib/dtrace/node.d

brew uninstall node
```

결국 node를 삭제하는 방법들을 몇 개 더 찾아서 시도해보고, 아래의 방법으로 성공할 수 있었다.

> 👍 node 삭제 성공

```bash
sudo rm -rf ~/.npm ~/.nvm ~/node_modules ~/.node-gyp ~/.npmrc ~/.node_repl_history
sudo rm -rf /usr/local/bin/npm /usr/local/bin/node-debug /usr/local/bin/node /usr/local/bin/node-gyp
sudo rm -rf /usr/local/share/man/man1/node* /usr/local/share/man/man1/npm*
sudo rm -rf /usr/local/include/node /usr/local/include/node_modules
sudo rm -rf /usr/local/lib/node /usr/local/lib/node_modules /usr/local/lib/dtrace/node.d
sudo rm -rf /opt/local/include/node /opt/local/bin/node /opt/local/lib/node
sudo rm -rf /usr/local/share/doc/node
sudo rm -rf /usr/local/share/systemtap/tapset/node.stp

brew uninstall node
brew doctor
brew cleanup --prune-prefix
```

## node 재설치

node를 설치하는 방법은 매우 쉽다. [node.js 공식 사이트](https://nodejs.org/ko/)에 들어가서 다운로드받으면 되는데, 가능하면 LTS로 추천한다.

## (MAC) node install 권한 오류

위 방법으로 node를 삭제하고, 재설치까지 했으나 이번에는 새로운 문제가 발생했다. `npm` 를 이용해 전역(`-g`)으로 설치할 경우 아래와 같은 에러가 발생했다.

![nodejs](/assets/images/etc/nodejs.png)

구글링 결과 전역으로 설치할 경우 발생하는 node install 권한 오류였다. 이 경우 명령어 앞에 `sudo` 를 붙여서 관리자 권한으로 실행하면 정상적으로 설치할 수 있다. 이 때 password를 물어보면 맥북 로그인 비밀번호를 입력해주면 된다. (가끔가다 보면 무슨 password 인지 헷갈리더라..ㅎㅎ)

# Ref.

- [Node JS 완전삭제](https://philosopher-chan.tistory.com/845)

- [How do I completely uninstall Node.js, and reinstall from beginning (Mac OS X)](https://stackoverflow.com/questions/11177954/how-do-i-completely-uninstall-node-js-and-reinstall-from-beginning-mac-os-x/26919540)

- [[TIP] MAC에서 node install 시 권한 오류 발생 할때](https://blog.sonim1.com/125)
