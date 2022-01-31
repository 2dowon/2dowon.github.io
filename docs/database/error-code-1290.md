---
layout: default
title: Error Code 1290. 파일 입출력 경로 설정 (secure-file-priv)
last_modified_date: 2021-01-06 16:01:66
parent: MySQL
grand_parent: Database
---

# Error Code 1290. 파일 입출력 경로 설정 (secure-file-priv)

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

Error Code : 1290. The MySQL server is running with the --secure-file-priv option so it cannot execute this statement

이 에러는 파일의 입출력 경로가 다를 경우에 발생하는 에러이다. MySQL 5.1.17 이후부터 보안을 위해서 파일 입/출력 전용 경로를 지정할 수 있기 때문이다. 보통은 (Window 기준) C:/ProgramData/MySQL/MySQL Server 8.0/Uploads 이 경로가 기본 설정되어 있는데, 이 경로가 수정되었거나 다른 경로에 입/출력을 원할 경우에는 경로를 (Window)my.ini 또는 (Mac)my.cnf 파일을 통해 secure-file-priv를 수정하면 해결할 수 있다.

# Window - my.ini 파일 수정하기

### 1. 명령 프롬프트를 관리자 권한으로 실행해서 my.ini 파일 열기

![csv2](/assets/images/database/csv2.png)

### 2. secure-file-priv 수정하기

secure-file-priv이 아래 사진과 같다면 아래 경로처럼 :/ProgramData/MySQL/MySQL Server 8.0/Uploads 폴더 안에 입력하고자 하는 데이터 파일이 있으면 된다. 다른 경로에 있는 파일도 자유롭게 입력하고 싶은 경우에는 아예 아예 경로를 지정하지 않으면 된다. 이 경우, 쌍따옴표 안에 아무것도 작성하지 않으면 된다.

⚠️ 만약 옵션을 여러 번 쓰는 경우 가장 마지막의 경로만 적용되므로 조심!

![csv3](/assets/images/database/csv3.png)

### 3. my.ini 파일을 저장하고 MySQL 서버를 재시작하기

![csv4](/assets/images/database/csv4.png)

# Mac - my.cnf 파일 수정하기

### 1. my.cnf 파일 찾기

Mac은 Window와 다르게 my.ini 파일이 아닌 my.cnf 파일이다. 이 파일을 수정해야 하는데 보통 이 파일의 경로는 설정에 들어가면 MySQL이 설치되어있다. 거기 들어가면 MySQL 서버를 시작하거나 중지할 수 있고, Configuration 탭에서는 아래처럼 그 경로를 확인할 수 있다.

![error1290_1](/assets/images/database/error1290_1.png)

### 2. my.cnf 파일에서 secure-file-priv 수정하기

위 방법으로 경로를 찾은 후 my.cnf 파일을 텍스트 편집기로 열어서 secure-file-priv 부분을 수정해주면 된다. 파일을 입출력하고자 하는 경로를 직접 적어도 좋고, 나는 경로를 아예 지정하지 않는 방법을 선택했다. 아래 사진처럼 `secure-file-priv = ""` 이 부분을 적으면 되는데 쌍따옴표 안에 원하는 경로를 적거나 지정하지 않을 경우 빈칸으로 두면 된다.

![error1290_2](/assets/images/database/error1290_2.png)

### 3. MySQL 서버 재시작하기

my.cnf 파일을 수정한 후에는 MySQL 서버를 재시작해서 적용한다. MySQL 서버를 재시작하는 방법은 터미널에서 `mysql.server stop` 으로 중지시키고 `mysql.server start` 로 시작하면 된다.
![error1290_3](/assets/images/database/error1290_3.png)

# 그래도 해결하지 못했다면

**Table Data Import Wizard** 기능을 이용하면 secure-file-priv를 수정하지 않아도 데이터 파일을 테이블에 넣을 수 있다. Workbench을 실행한 후 원하는 데이터베이스를 선택하고 아래 있는 Tables를 클릭한 후 오른쪽 버튼을 누르면 아래처럼 보인다

![error1290_4](/assets/images/database/error1290_4.png)

이 중에서 Table Data Import Wizard 를 선택하면 아래처럼 직접 파일을 브라우저에서 선택할 수 있다.

![error1290_5](/assets/images/database/error1290_5.png)

단, 이 경우에는 데이터 파일에 들어있는 데이터로만 일단 테이블이 만들어진다. 따라서 데이터를 활용하여 새로운 column을 만들고 싶다면 `ALTER TABLE 테이블명 ADD COLUMN 칼럼명;` 을 이용해 column을 먼저 새로 만든 후에 UPDATE문으로 데이터를 입력하면 된다. [csv파일 Import하는 법](https://2dowon.netlify.app/database/mysql---import-csv-file-to-table/)에 대해 작성한 이 글을 참고!

# Ref.

- [[코끼리를 냉장고에 넣는 방법] MYSQL 파일 입출력 경로 바꿔주기](https://dololak.tistory.com/252)
