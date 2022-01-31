---
layout: default
title: Data too long for column
last_modified_date: 2021-01-07 22:01:82
parent: MySQL
grand_parent: Database
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Data too long for column</div>

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

MySQL에서 데이터를 INSERT할 때 종종 Data too long for column 이 에러를 만나게 된다.

이 에러는 column의 선언된 크기보다 INSERT할 데이터의 크기가 더 클 때 발생한다. 예를 들어, 전화번호의 데이터 크기를 11자리로 선언했는데, 12자리로 잘 못 입력하는 경우에 이와 같은 에러가 발생할 수 있다.

## Data too long for column 에러 해결 방법

### 1. 입력하고자 하는 데이터 또는 column 수정하기

보통 이 에러는 입력하고자 하는 데이터가 잘못되었을 가능성이 높다. **이 에러가 발생했다면 먼저 입력하고자 하는 데이터를 확인해보자.** 입력하고자 하는 데이터를 분석해서 데이터가 잘못되었다면 데이터를 수정하고, column의 크기를 잘못 선언했다면 column을 수정하는 것이 좋다.

### 2. sql_mode의 strict mode를 비활성화하기

입력하고자 하는 데이터를 확인했음에도 문제가 없거나 아니면 수정할 수 없는 경우라면 sql mode의 strict mode를 비활성화시킴으로써 해결할 수 있다.

sql mode의 strict mode를 비활성화하게 되면 column에 선언된 크기와 INSERT할 데이터의 크기가 달라도 에러가 발생하지 않게 된다.

✅ MySQL 5.x 버전 이상부터는 sql mode가 strict mode로 기본 설정되어 있다. strict mode는 MySQL 데이터베이스에서 유효하지 않거나 누락된 데이터를 처리하는 방법으로 데이터베이스를 안정성 있게 관리해주는 모드이다. **따라서 strict mode를 비활성화시키는 방법보다는 위에 말한 것처럼 잘못된 부분을 수정하는 것을 더 추천한다.**

### strict mode 비활성화하는 법

(Window)my.ini 또는 (Mac)my.cnf 파일에서 sql mode 부분을 수정해주면 된다.

sql mode는 기본값으로 아래처럼 "STRICT TRANS TABLES, NO ENGINE SUBSTITUTION" 이라고 적혀 있다.

![data_long1](/assets/images/database/data_long1.png)

이 부분에서 아래처럼 STRICT_TRANS_TABLES를 삭제한 후 저장하면 된다.

![data_long2](/assets/images/database/data_long2.png)

✅ (Window)my.ini 또는 (Mac)my.cnf 파일을 수정하는 방법은 [이 글](https://2dowon.netlify.app/database/error-code-1290/)을 참고!

# Ref.

- [[꼬마개발자의 블로그] Data too long for column 에러 해결 방법 : sql_mode변경](https://m.blog.naver.com/PostView.nhn?blogId=devks0228&logNo=221637966872&proxyReferer=https:%2F%2Fwww.google.com%2F)
