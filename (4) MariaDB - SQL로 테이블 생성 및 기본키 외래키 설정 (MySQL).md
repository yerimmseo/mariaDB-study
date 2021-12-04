## MariaDB - SQL로 테이블 생성 및 기본키 외래키 설정 (MySQL)

> **userTBL 테이블 생성**

*[테이블 명세서]*

userID char(8) NOT NULL PRIMARY KEY, -- 사용자 아이디

name nvarchar(10) NOT NULL, -- 이름

birthYear int NOT NULL, -- 출생년도

addr nchar(2) NOT NULL, -- 지역(글자만 입력)

mobile1 char(3) NULL, -- 휴대폰의 국번(011, 016, 017, 018, 019, 010 ...)

mobile2 char(8) NULL, -- 휴대폰의 나머지 전화번호(하이픈 제외)

height smallint NULL, -- 키

mDate date NULL -- 회원 가입일

```mysql
DROP TABLE IF EXISTS userTBL; -- userTBL 테이블이 존재한다면 삭제(DROP)
CREATE TABLE userTBL -- userTBL 테이블 생성
(userID char(8) NOT NULL PRIMARY KEY, -- 기본키
 name varchar(10) NOT NULL,
 birthYear int NOT NULL,
 addr char(2) NOT NULL,
 mobile1 char(3) NULL, -- 널값 허용
 mobile2 char(8) NULL,
 height smallint NULL,
 mDate date NULL
);
```





> **buyTBL 테이블 생성**

*[테이블 명세서]*

num int AUTO_INCREMENT NOT NULL PRIMARY KEY, -- 순번(기본키, 자동 증가)

userID char(8) NOT NULL, -- 아이디(외래키)

prodName char(6) NOT NULL, -- 물품명

groupName char(4) NULL, -- 분류

price int NOT NULL, -- 단가

amount smallint NOT NULL, -- 수량

FOREIGN KEY (userID) REFERENCES userTBL (userID) -- buyTBL의 외래키와 userTBL의 기본키를 연결

```mysql
DROP TABLE IF EXISTS buyTBL; -- buyTBL 테이블이 존재하면 삭제(DROP)
CREATE TABLE buyTBL -- buyTBL 테이블 생성
(num int AUTO_INCREMENT NOT NULL PRIMARY KEY, -- 자동 상승, NOT NULL, 기본키 설정(자동 상승은 기본키만 가능)
 userID char(8) NOT NULL,
 prodName char(6) NOT NULL,
 groupName char(4) NULL, -- 널값 허용
 price int NOT NULL, 
 amount smallint NOT NULL,
 FOREIGN KEY (userID) REFERENCES userTBL (userID)
);
```





> **자료 입력하기**

```mysql
INSERT INTO userTBL VALUES('aaa', N'에이', 1997, N'서울', '010', '111111', 185, '2007-08-08');
INSERT INTO userTBL VALUES('bbb', N'비', 1999, N'경남', '010', '2222222', 175, '2011-04-04');
INSERT INTO userTBL VALUES('ccc', N'씨', 1991, N'전남', '010', '3333333', 176, '2008-07-07');
```

buyTBL 테이블에 아래 쿼리를 입력할 때 외래키-기본키 연결 상태이기 때문에,

buyTBL 외래키 (userID)는 반드시 userTBL에 입력되어 있는 aaa, bbb, ccc 중에서 값을 입력.

```mysql
INSERT INTO buyTBL VALUES(NULL, 'aaa', N'운동화', NULL, 30, 2) -- 정상 입력
INSERT INTO buyTBL VALUES(NULL, 'bbb', N'노트북', N'전자', 1000, 1) -- 정상
INSERT INTO buyTBL VALUES(NULL, 'ddd', N'모니터', N'전자', 200, 1) -- 오류 발생
```

