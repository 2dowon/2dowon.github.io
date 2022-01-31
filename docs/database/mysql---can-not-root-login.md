---
layout: default
title: root 로그인이 안될 때
last_modified_date: 2020-12-19 21:12:20
parent: MySQL
grand_parent: Database
---

# root 로그인이 안될 때

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

# MAC에서 MySQL 설치하기

학원에서 MySQL을 배울 때는 윈도우 환경에서 배웠는데, 평소에는 맥북을 사용하다보니 MySQL을 따로 공부하기 위해서 맥북에도 MySQL을 설치했다. macOS용 패키지 관리자 Homebrew를 이용해서 아래 명령어를 통해 MySQL을 설치했다.

```python
brew list
brew install mysql
```

# MySQL root 사용자 로그인 에러 발생

MySQL을 설치한 후에는 기본적인 설정이 필요해서 [이 포스팅](https://whitepaek.tistory.com/16)을 참고해서 MySQL을 설정했다. 그리고 맥북 설정 - MySQL에서 직접 서버를 시작한 후, root 로그인을 하려고 하는데 로그인이 안되는 에러가 발생했다.

![mysql_login1](/assets/images/database/mysql_login1.png)

위 사진처럼 MySQL Server가 켜진 것을 확인하고 `mysql -u root -p` 명령어를 이용해 root 계정으로 로그인하려고 했으나 비밀번호가 맞지 않다는 이유로 아래처럼 계속 에러가 발생했다.

![mysql_login2](/assets/images/database/mysql_login2.png)

# MySQL root 사용자 로그인 에러 해결

그래서 MySQL 비밀번호를 재설정해야 되나 계속 정보를 찾던 중에 아주 손쉽게 에러를 해결했다. MySQL 서버를 저렇게 맥북 설정을 통해서 켜는 것이 아니라 터미널에서 `mysql.server start` 라는 명령어를 통해서 켜야 제대로 root계정으로 로그인할 수 있었다.

아래 사진을 보면 처음에 바로 로그인을 시도했을 때는 Access denied for user 'root'@'localhost' (using password: YES) 라는 에러가 나왔지만, 직접 MySQL 서버를 시작한 후에 똑같이 시도하자 이번에는 성공한 것을 확인할 수 있다.

![mysql_login3](/assets/images/database/mysql_login3.png)

# MySQL Workbench

아, 그리고 MySQL 서버가 켜져 있어야 MySQL Workbench에서도 local host 데이터베이스에 접근할 수 있다. 윈도우 환경에서는 설치만으로도 사용이 가능했기에 간과한 부분이었는데, 맥 환경에서는 좀 더 신경써야겠다.

MySQL Workbench는 [여기서](https://dev.mysql.com/downloads/workbench/) 다운로드 받을 수 있다.

![mysql_login5](/assets/images/database/mysql_login5.png)

# MySQL 기본 명령어

- `mysql.server start` 서버를 시작하는 명령어로 제일 먼저 해야 한다
- `mysql -u root -p` root 사용자로 데이터베이스에 접속하기 위한 명령어
- `mysql -u 사용자명 -p` 만약 root가 아닌 다른 사용자 데이터베이스라면 root 대신 입력
- `exit` 또는 `quit` 명령어를 이용하면 mysql 쉘에서 로그아웃할 수 있다
- `mysql.server stop` 마지막으로 MySQL 서버를 종료
