### [MariaDB]  JOIN (내부 조인)

> #### INNER JOIN (내부 조인)

N:1의 관계인 테이블1:테이블2를 인용하는 조건

테이블1(N)의 외래키와 테이블2(1)의 기본키를 조인될 조건으로 이용하여 두 TABLE을 합침

예) 테이블1= 주문테이블, 테이블2= 개인정보(주소)테이블, 테이블1에 테이블2를 JOIN하여 주문자의 주소를 알아내는 기능을 함

```mariadb
USE 주문판매DB;
SELECT * FROM 주문테이블명 INNER JOIN 개인정보테이블명 ON 주문테이블명.userID(외래키) = 개인정보테이블명.userID(기본키) WHERE 주문테이블명.userID = '주문자ID';
```



전체를 SELECT 하지 않고 일부만 SELECT 하고자 할 경우는 어떤 테이블의 열이름인지 확실히 해주어야 함

```mariadb
USE 주문판매DB;
SELECT 주문테이블명.userID, 주소 FROM 주문테이블명 INNER JOIN 개인정보테이블명 ON 주문테이블명.userID(외래키) = 개인정보테이블명.userID(기본키) WHERE 주문테이블명.userID = '주문자ID';
```



각 테이블을 별칭으로 만들어 사용 가능

```mariadb
USE 주문판매DB;
SELECT B.userID, U.주소 FROM 주문테이블명 B INNER JOIN 개인정보테이블명 U ON B.userID(외래키) = U.userID(기본키) WHERE 주문테이블명.userID = '주문자ID';
```





> OUTER JOIN (외부 조인)

외부 조인은 테이블1의 모든 행이 조회됨. 주문내역이 없는 개인정보테이블의 행까지 조회됨

```mariadb
USE 주문판매DB;
SELECT * FROM 개인정보테이블명 LEFT OUTER JOIN 주문테이블명 ON 개인정보테이블.userID(기본키) = 주문테이블명.userID(외래키) ORDER BY 개인정보테이블.userID;
```





> CROSS JOIN (상호 조인)

테이블1의 모든 행들을 테이블2의 모든 행들과 조인하는 기능

예) 테이블1의 행 5개, 테이블2의 행 10개일 경우 5 * 10 = 50개의 행을 가진 상호 조인 테이블 생성

보통 큰 샘플 데이터를 생성하고자 할 때 사용하는 조인

```mariadb
USE 주문판매DB;
SELECT * FROM 테이블1 CROSS JOIN 테이블2;
```





> SELF JOIN (자체 조인)

내부 조인을 이용해서 만들지만 별칭을 달리하여 다른 행들의 열 값을 SELECT 해서 가져옴

```mariadb
SELECT A.본인이름, A.상사이름, B.상사이름 AS '상사의 상사이름' FROM 중대원테이블A INNER JOIN 중대원테이블B ON A.상관이름 = B.본인이름 WHERE A.본인이름 = '이름명';
```





> UNION / UNION ALL

데이터 형식이 같은 두 테이블이 있다면 UNION시 테이블1 마지막 행 밑에 테이블2가 붙여짐

UNION은 중복된 열 제거, UNION ALL은 중복된 열까지 모두 출력

```mariadb
USE DB명;
SELECT 열이름1, 열이름2 FROM 테이블1 UNION ALL SELECT 열이름1, 열이름2 FROM 테이블2;
```





> NOT IN / IN

NOT IN은 첫번째 쿼리 결과 중에서 두번째 쿼리에 해당하는 행을 제외하고 출력

```mariadb
SELECT * FROM 개인정보테이블 WHERE 이름열 NOT IN (SELECT 이름열 FROM 개인정보테이블 WHERE 모바일열 IS NULL);
-- 개인정보테이블의 이름열 중에 모바일열이 NULL인 이름열들은 제외
```

IN은 첫번째 쿼리 결과 중에서 두번째 쿼리에 해당하는 행들만 출력

```mariadb
SELECT * FROM 개인정보테이블 WHERE 이름열 IN (SELECT 이름열 FROM 개인정보테이블 WHERE 모바일열 IS NULL);
-- 개인정보테이블의 이름열 중에 모바일열이 NULL인 이름열들만 출력
```







### [MariaDB] SQL 프로그래밍

> #### IF ELSE

```mariadb
DROP PROCEDURE IF EXISTS ifProc; -- 기존에 프로시저가 존재한다면 삭제
DELIMITER $$
CREATE PROCEDURE ifProc(); -- ifProc() 프로시저 생성
BEGIN
	DECLARE var1 INT; -- var1 변수 선언
	SET var1 = 100; -- 변수에 값 대입
	
	IF var1 = 100 THEN -- 만약 @var1이 100이라면
		SELECT '100입니다.';
	ELSE
		SELECT '100이 아닙니다.';
	END IF; -- IF 끝
END $$
DELIMITER ;
CALL ifProc(); -- 프로시저 호출하여 결과값 '100입니다.'출력
```

* DELIMITER : C나 JAVA의 세미콜론(;), 문법의 끝을 나타내는 역할. 구문 문자를 정의하는 기능을 함
  DELIMITER 명령어 뒤에 구문 문자로 사용하고자 하는 문자를 넣어주면 됨

  ```mariadb
  DELIMITER $$
  SELECT * FROM account $$
  ```

  ```mariadb
  DELIMITER $$
  CREATE PROCEDURE insert_test()
  BEGIN
  DECLARE i INT;
  SET i = 0;
  WHILE i < 100000 DO
  INSERT INTO accoint(createDatetime) VALUES(now());
  SET i = i + 1;
  END WHILE;
  END $$
  DELIMITER ;
  ```

  * PROCEDURE을 정의할 때 내부에 세미콜론을 사용함. 만약 DELIMITER를 설정하지 않으면 문장을 구분하기가 어렵기 때문에 DELIMITER를 세미콜론이 아닌 $$로 설정
  * 또한 마지막에 DELIMITER 명령어를 다시 사용한 이유는 다시 세미콜론을 이용하여 문장을 구분하기 위해서이며 PROCEDURE정의에 필수적인 요소는 아닌 셈





> #### IF ELSEIF ELSE

```mariadb
DROP PROCEDURE IF EXISTS ifProc();
DELIMITER $$
CREATE PROCEDURE ifProc();
BEGIN
	DECLARE point INT;
	DECLARE credit CHAR(1);
	SET point = 77;
	
	IF point >= 90 THEN
		SET credit = 'A';
	ELSEIF point >= 80 THEN
		SET credit = 'B';
	ELSEIF point >= 70 THEN
		SET credit = 'C';
	ELSEIF point >= 60 THEN
		SET credit = 'D';
	ELSE 
		SET credit = 'F';
	END IF;
	SELECT CONCAT('취득점수==>', point), CONCAT('학점==>', credit); -- point, credit 출력
END $$
DELIMITER ;
CALL ifProc();
```





> #### CASE

```mariadb
DROP PROCEDURE IF EXISTS caseProc();
DELIMITER $$
CREATE PROCEDURE caseProc()
BEGIN 
	DECLARE point INT;
	DECLARE credit CHAR(1);
	SET point = 77;
	
	CASE
		WHEN point >= 90 THEN
			SET credit = 'A';
		WHEN point >= 80 THEN
			SET credit = 'B';
		WHEN point >= 70 THEN
			SET credit = 'C';
		WHEN point >= 60 THEN
			SET credit = 'D';
		ELSE
			SET credit = 'F';
	END CASE;
	SELECT CONCAT('취득점수==>', point), CONCAT('학점==>', credit);
END $$
DELIMITER ;
CALL caseProc();
```





> #### WHILE

```mariadb
DROP PROCEDURE IF EXISTS whileProc();
DELIMITER $$
CREATE PROCEDURE whileProc()
BEGIN
	DECLARE i INT;
	DECLARE hap INT;
	SET i = 1;
	SET hap = 0;
	
	WHILE (i <= 100) DO -- WHILE문 시작
		SET hap = hap + i;
		SET i = i + 1;
	END WHILE;
	
	SELECT hap;
END $$
DELIMITER ;
CALL whilProc();
```





> #### WHILE INTERATE / LEAVE

```mariadb
DROP PROCEDURE IF EXISTS whileProc();
DELIMITER $$
CREATE PROCEDURE whileProc()
BEGIN
	DECLARE i INT;
	DECLARE hap INT;
	SET i = 1;
	SET hap = 0;
	
	myWhile: WHILE (i <= 100) DO -- While문에 label(myWhile)을 지정. 
								 -- i가 100보다 같거나 작으면 작동. 크면 while문 벗어남.
		IF (i % 7 = 0) THEN
			SET i = i + 1;
			ITERATE myWhile; -- 지정한 label문으로 가서 계속 진행. 
							 -- 아래는 실행하지 않고 바로 While로 돌아감.
		END IF;
		
		SET hap = hap = i;
		IF (hap > 1000) THEN -- hap이 1000보다 커지면
			LEAVE myWhile; -- 지정한 label문을 떠남. While 종료.
		END IF;
		SET i = i + 1;
	END WHILE;
	
	SELECT hap; -- hap 조회
END $$
DELIMITER ;
CALL whileProc();
```





> #### 오류처리 (DECLARE CONTINUE HANDLER FOR 에러코드 에러시작업내역;)

```mariadb
DROP PROCEDURE IF EXISTS errorProc();
DELIMITER $$
CREATE PROCEDURE errorProc()
BEGIN
	DECLARE CONTINUE HANDLER FOR 1146 SELECT '테이블 없음' AS '메시지'; -- 아래 SELECT문이 1146에러 시 '테이블 없음'을 출력
	SELECT * FROM noTable; -- noTable은 없는 테이블로 위에 예외처리를 발생시킴
END $$
DELIMITER ;
CALL errorProc();
```

```mysql
DROP PROCEDURE IF EXISTS errorProc();
DELIMITER $$
CREATE PROCEDURE errorProc()
BEGIN
	DECLARE CONTINUE HANDLER FOR SQLEXCEPTION -- 아래 INSERT에서 SQLEXCEPTION 에러 발생시 BEGIN ~ END 실행하는 예외처리
	BEGIN
		SHOW ERRORS; -- 오류 메세지를 보여줌
		SELECT '오류발생. 롤백합니다.' AS '메시지'; -- 오류 발생시 '오류발생. 롤백합니다.' 출력
		ROLLBACK;
	END;
	INSERT INTO userTBL VALUES('ID1004', '김이박', 1990, '서울', NULL,
                              NULL, 180, CURRENT_DATE()); -- 'ID1004' 값이 중복되는 아이디로 가정하여 위 예외처리 발생
END $$
DELIMITER ;
CALL errorProc(); -- '오류발생. 롤백합니다.' 출력 및 오류 메세지 출력 및 롤백 작업.
```





> #### 동적SQL (PREPARE / EXECUTE / DEALLOCATE PREPARE)

```mariadb
USE DB명;
PREPARE 쿼리명 FROM 'SELECT * FROM 테이블'; -- SELECT 문을 '쿼리명'으로 준비시킴
EXECUTE 쿼리명;
DEALLOCATE PREPARE 쿼리명; -- '쿼리명'의 준비를 해제시킴
```

```mariadb
USE DB명;
DROP TABLE IF EXISTS 테이블명; -- 테이블이 존재한다면 지워줌
CREATE TABLE 테이블명 (id INT AUTO_INCREMENT PRIMARY KEY, mDate DATETIME);

SET @curDATE = CURRENT_TIMESTAMP(); -- @curDATE에 현재 날짜와 시간 입력

PREPARE 쿼리명 FROM 'INSERT INTO myTABLE VALUES (NULL, ?)'; -- 쿼리명 준비. '?'아래에서 정의
EXECUTE 쿼리명 USING @curDATE; -- 쿼리명 실행. ?에는 @curDATE 변수를 대입함.
DEALLOCATE PREPARE 쿼리명; -- 쿼리명 해제

SELECT * FROM 테이블명;
```





































