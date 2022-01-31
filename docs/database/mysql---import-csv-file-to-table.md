---
layout: default
title: csv파일 Import하기
last_modified_date: 2021-01-05 23:01:05
parent: MySQL
grand_parent: Database
---

# csv파일 Import하기

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

학원에서 오늘의 숙제 느낌으로 구글 스프레드 시트에 있는 수강생 리스트로 데이터베이스를 만들게 되었다. 사실 수강생 리스트가 많진 않아서 하나 하나 데이터를 insert해도 되지만, 개인적으로 csv파일을 바로 데이터베이스의 테이블로 Import하고 싶었다.

# LOAD DATA INFILE로 테이블에 데이터 넣기

## 1. 데이터베이스와 테이블 만들기

```sql
CREATE DATABASE playdata;
USE playdata;

CREATE TABLE member -- Playdata memeber 테이블
( num	INT UNSIGNED AUTO_INCREMENT NOT NULL PRIMARY KEY, -- 고유 순번(PK)
  name		CHAR(3) NOT NULL, -- 이름
  gender	ENUM('M', 'F') NOT NULL,
  mobile	char(13), -- 휴대폰 번호
  firstNum	CHAR(3), -- 휴대폰의 국번 010
  midNum 	CHAR(4), -- 휴대폰의 중간 4자리 번호
  lastNum 	CHAR(4), -- 휴대폰의 마지막 4자리 번호
  addr		CHAR(10), -- 주소
  addr1		CHAR(5), -- 시
  addr2		CHAR(5), -- 구
  email		CHAR(30), -- 이메일
  email1	char(10), -- 이메일 아이디
  email2	char(20), -- 계정 ex. naver.com
  major		char(10), -- 전공
  birthYear	INT -- 출생년도
);
```

구글 스프레드 시트에 저장되어 있는 정보에는 순번, 이름, 성별, 전화번호, 주소, 이메일 주소, 전공, 년생(ex. 95년생) 이렇게 있다. 이 정보들을 가져오는데 전화번호의 경우 첫 3자리, 중간 4자리, 마지막 4자리를 나누고 이메일도 @를 기준으로 앞 뒤로 나누고, 주소도 시와 구로 나눠서 저장하는 것이 조건이어서 테이블을 만들 때 num, name, gender, mobile, addr, email, major, birthYear 외에도 firstNum, midNum, lastNum, addr1, addr2, email1, email2 열을 추가로 만들었다.

## 2. csv파일을 테이블로 Import 하기

```sql
LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/playdata.csv'
INTO TABLE member
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(@num,@name,@gender,@mobile,@addr,@email,@major,@birthYear)
SET name = @name,
 gender = @gender,
 mobile = @mobile,
 firstNum = SUBSTRING_INDEX(@mobile, '-', 1),
 midNum = SUBSTRING(@mobile, 5, 4),
 lastNum = SUBSTRING_INDEX(@mobile, '-', -1),
 addr = @addr,
 addr1 = SUBSTRING_INDEX(@addr, ' ', 1),
 addr2 = SUBSTRING_INDEX(@addr, ' ', -1),
 email = @email,
 email1 = SUBSTRING_INDEX(@email, '@', 1),
 email2 = SUBSTRING_INDEX(@email, '@', -1),
 major = @major,
 birthYear = CONCAT(19, REPLACE(@birthYear, '년생', ''));
```

구글 스프레드 시트에 저장되어 있는 수강생 리스트를 playdata.csv 라는 이름의 파일로 저장한 후 C:/ProgramData/MySQL/MySQL Server 8.0/Uploads 이 경로에 넣었다. 파일을 업로드하는 경우의 경로로는 이 경로가 기본 설정되어있지만, 나는 다른 실습으로 인해서 이 경로가 바뀌어서 Error Code 1290이 발생했다. 해결방법은 아래에서 확인!

- `LOAD DATA INFILE '파일 경로'` : 이 경로에서 파일을 가져온다
- `INTO TABLE 테이블명` : 데이터를 넣고자 하는 테이블 이름을 적는다
- `FIELDS TERMINATED BY ','` : csv 파일이므로 구분자는 콤마(,)이다
- `ENCLOSED BY '"'` : 따옴표(")는 무시한다
- `LINES TERMINATED BY '\n'` : 줄바꿈 표시가 줄이 끝났다는 뜻이다
- `IGNORE 1 ROWS` : csv 파일의 첫 번째 줄에는 열 이름이 있으므로 이는 무시한다

firstNum, midNum, lastNum, addr1, addr2, email1, email2 은 csv 파일에 데이터가 없기 때문에 기존 데이터를 이용해서 입력해야 한다. SUBSTRING_INDEX, REPLACE 등 함수를 이용해서 넣기!

사실 csv파일을 테이블로 Import 하는 것은 MySQL Workbench에서 Table Data Import Wizard로 더 쉽게 할 수 있는데, `LOAD DATA INFILE` 문법을 연습해보고 싶어서 이렇게 진행했다. 근데 Mac에서 복습할 때는 1290 에러를 해결하지 못해서 결국 Table Data Import Wizard를 이용했는데, 이는 아래에서 확인할 수 있다.

### playdata.csv 파일 예시

개인정보가 많이 포함된 파일이라 원본 데이터를 공개하지는 못하지만, 아래 사진처럼 첫 줄에는 칼럼 명이 적혀있고, 두번째 줄부터 해당 데이터가 콤마(,)로 구분되어 저장되어 있다.

![csv1](/assets/images/database/csv1.png)

## Error Code : 1290 해결하기

MySQL 5.1.17 이후부터 보안을 위해서 파일 입/출력 전용 경로를 지정할 수 있다고 한다. 보통은 C:/ProgramData/MySQL/MySQL Server 8.0/Uploads 이 경로가 기본 설정되어 있는데, 이 경로가 수정되었거나 다른 경로에 입/출력을 원할 경우에는 경로를 my.ini 파일을 통해 수정할 수 있다.

> Window 환경에서 my.ini 파일 수정하기

1. 명령 프롬프트를 관리자 권한으로 실행해서 my.ini 파일 열기
   ![csv2](/assets/images/database/csv2.png)

2. Secure File Priv 부분을 찾아서 아래처럼 되어있는지 확인하기
   ![csv3](/assets/images/database/csv3.png)

   만약 옵션을 여러 번 쓰는 경우 가장 마지막의 경로만 적용되므로 조심!

3. my.ini 파일을 저장하고 MySQL 서버를 재시작하기
   ![csv4](/assets/images/database/csv4.png)

## Error Code : 1406 - Data too long 해결하기

데이터베이스에 지정해 놓은 길이보다 데이터의 길이가 더 길 경우에 발생하는 에러이다. column의 데이터 크기를 직접 수정해줘도 가능하지만, strict mode를 비활성화시키는 방법으로도 해결할 수 있다. strict mode는 MySQL 5.x 버전부터 기본값으로 적용되었는데, 데이터베이스를 안정성 있게 관리해주는 모드이다. 따라서 개인적으로는 1406 에러가 발생하면 그때 데이터베이스를 수정하는 것을 추천한다.

그래도 strict mode를 비활성화 시키는 법을 알아보면 1290 에러를 해결하듯이 my.ini 파일을 수정해야 한다. my.ini 파일에서 sql-mode를 찾아 STRICT_TRANS_TABLES를 지워주면 끝! my.ini 파일을 수정 할 때는 반드시 명령 프롬프트를 관리자 권한으로 실행하고, 수정 후에는 MySQL 서버를 재시작해야 한다.

# Table Data Import Wizard로 테이블에 데이터 넣기

Mac에서는 Error Code : 1290을 해결하지 못해서 **MySQL Workbench에서 Table Data Import Wizard** 를 이용해 CSV 파일을 Import 했다. 데이터베이스는 미리 만들어야 하고, 테이블은 Table Data Import Wizard로 데이터를 넣을 때 만들면 된다.

이 경우에는 csv 파일에 있는 데이터로 테이블이 만들어지기 때문에 firstNum, midNum, lastNum, addr1, addr2, email1, email2 처럼 csv파일에 없는 데이터는 테이블에 따로 column을 추가한 뒤 기존 데이터를 이용해 update하면 된다.

TIP. csv파일의 첫 줄에 column 명이 적혀있다면 자동으로 그 이름으로 column명이 만들어진다.

```sql
CREATE DATABASE playdata;
USE playdata;

-- palydata의 Tables 오른쪽 버튼 Table Data Import Wizard를 통해 csv파일 import하기

-- num을 1부터 시작할 수 있도록 (기존 데이터는 0부터 시작)
SET SQL_SAFE_UPDATES = 0; -- 일시적으로 safe_mode  해제하기
UPDATE member SET num = NULL;
ALTER TABLE member MODIFY COLUMN num INT UNSIGNED AUTO_INCREMENT NOT NULL PRIMARY KEY;

-- 전화번호 분리
ALTER TABLE member ADD COLUMN firstNum char(3);
UPDATE member SET firstNum = SUBSTRING_INDEX(mobile, "-" ,1);

ALTER TABLE member ADD COLUMN midNum char(4);
UPDATE member SET midNum = SUBSTRING(mobile, 5, 4);

ALTER TABLE member ADD COLUMN lastNum char(4);
ALTER TABLE member MODIFY COLUMN lastNum char(10); -- Data too long
UPDATE member SET lastNum = SUBSTRING_INDEX(mobile, "-" ,-1);

-- 이메일 분리
ALTER TABLE member ADD COLUMN email1 char(10);
ALTER TABLE member MODIFY COLUMN email1 char(20); -- Data too long
UPDATE member SET email1 = SUBSTRING_INDEX(email,"@",1);

ALTER TABLE member ADD COLUMN email2 char(20);
UPDATE member SET email2 = SUBSTRING_INDEX(email,"@",-1);

-- 주소 분리
ALTER TABLE member ADD COLUMN addr1 char(5);
UPDATE member SET addr1 = SUBSTRING_INDEX(addr," ",1);

ALTER TABLE member ADD COLUMN addr2 char(10);
UPDATE member SET addr2 = SUBSTRING_INDEX(addr," ",-1);

-- 년생 제거하고 19 앞에 붙이기 ex. 95년생 -> 1995
UPDATE member SET birthYear = CONCAT(19, REPLACE(birthYear, '년생', ''));
-- 년생 값이 없는 경우 null로 저장
UPDATE member SET birthYear = null WHERE name = '신동욱';

SELECT * FROM playdata.member;
SELECT num, name, gender, firstNum, midNum, lastNum, addr1, addr2, email1, email2, major, birthYear FROM playdata.member;

-- 서울
SELECT name, addr1 AS '지역' FROM member
   WHERE addr1 IN ('서울시') ;

-- 전공자
SELECT name, major AS '전공' FROM member
   WHERE major IN ('컴퓨터공학과', '빅데이터', '수학과', '경영학과') ;
-- 전공자 수
SELECT COUNT(*) AS '전공자 수' FROM member
   WHERE major IN ('컴퓨터공학과', '빅데이터', '수학과', '경영학과') ;

-- 비전공자
SELECT name, major AS '비전공' FROM member
   WHERE major NOT IN ('컴퓨터공학과', '빅데이터', '수학과', '경영학과') ;
-- 비전공자 수
SELECT COUNT(*) AS '비전공자 수' FROM member
   WHERE major NOT IN ('컴퓨터공학과', '빅데이터', '수학과', '경영학과') ;
```

중간에 Data too long 에러가 발생했는데, 이 에러도 Window에서는 my.ini 파일에서 strict mode를 삭제하면 해결됐는데 Mac에서는 수정이 불가능해 칼럼 자체를 데이터 크기를 늘려서 수정해서 해결했다. (참고로 Data too long 에러는 csv파일의 데이터가 내가 지정한 데이터의 크기보다 더 큰 경우에 데이터가 잘리면서 발생한 것이다.)

# Ref.

- [[바부의 세상살아가기] csv파일을 직접 테이블로 import](https://cirius.tistory.com/1475)
- [[코끼리를 냉장고에 넣는 방법] MYSQL 파일 입출력 경로 바꿔주기](https://dololak.tistory.com/252)
- [[꼬마개발자의 블로그] Data too long for column 에러 해결 방법 : sql_mode변경](https://m.blog.naver.com/PostView.nhn?blogId=devks0228&logNo=221637966872&proxyReferer=https:%2F%2Fwww.google.com%2F)
