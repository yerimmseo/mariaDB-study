### MariaDB SQL 쿼리 - 
DB목록 조회

```mariadb
SHOW DATABASE;
```

DB선택

```mariadb
USE DB명;
```

선택된 DB에 존재하는 테이블 정보 조회

```mariadb
SHOW TABLE STATUS; -- 테이블 이름만 간한히 볼 때는 SHOW TABLES;
```

테이블 조회(SELECT)

```mariadb
SELECT * FROM 테이블명;
```

DB선택과 함께 테이블 조회

```mariadb
SELECT * FROM DB명.테이블명;
```

테이블에서 필요한 열만 조회

```mariadb
SELECT 열이름1, 열이름2, 열이름3 FROM 테이블명;
```

별칭 사용법(AS)

```mariadb
SELECT * 열이름1 AS 별칭1, 열이름2 AS 별칭2 FROM 테이블명; 
```

조건절 AND, OR

```mariadb
SELECT * FROM 테이블명 WHERE 조건1 AND 조건2;
SELECT * FROM 테이블명 WHERE 조건1 OR 조건2;
```

조건절 BETWEEN...AND

```mariadb
SELECT * FROM 테이블명 WHERE 열이름 BETWEEN 시작값 AND 끝값;
```

조건절 IN('값1', '값2', ...)

```mariadb
SELECT * FROM 테이블명 WHERE 열이름 IN ('값1', '값2', '값3');
```

조건절 LIKE '%'

```mariadb
SELECT * FROM 테이블명 WHERE 열이름 LIKE '김%'
```

순서정렬 (ORDER BY)

```mariadb
SELECT * FROM 테이블명 ORDER BY 열이름; -- 기본값이 ASC이며 열이름 기준 오름차순 정렬
SELECT * FROM 테이블명 ORDER BY 열이름 DESC;
```

중복제거 조회 (DISTINCT)

```mariadb
SELECT 지역명 FROM 동창회테이블;
SELECT DISTICT 지역명 FROM 동창회테이블; -- 조회하는 지역명 중 중복되는 값은 1개만 조회
```

조회(출력) 갯수 제한(LIMIT)

```mariadb
SELECT * FROM 테이블명 LIMIT 5; -- 5개 행까지만 조회
```

테이블 새이름으로 복사하기

```mariadb
/* DB명 안에 있는 기존테이블을 새 테이블로 복사 */
USE 테이블명;
CREATE TABLE 새테이블이름 (SELECT 복사할열1, 복사할열2 FROM 기존테이블);
```

GROUP BY절

```mariadb
/* 그룹열을 중복제거 및 조회하고 그룹열에 해당되는 합칠열 값을 모두 더해서(SUM) 출력 */
SELECT 그룹열, SUM(합칠열) FROM 테이블명 GROUP BY 그룹열;
SELECT 그룹열, SUM(수량열*가격열) FROM 테이블명 GROUP BY 그룹열;

-- Ordering Results
SELECT name, test, score FROM student_tests ORDER BY score DESC;
```

GROUP BY절과 함께 사용되는 집계함수

|     함수명      |                 설명                  |
| :-------------: | :-----------------------------------: |
|      AVG()      |              평균을 구함              |
|      MIN()      |              최소값 구함              |
|      MAX()      |              최대값 구함              |
|     COUNT()     |           행의 개수를 구함            |
| COUNT(DISTINCT) | 행의 개수를 구함(중복값은 1개로 인정) |
|     STDEV()     |            표준편차를 구함            |
|   VAR_SAMP()    |              분산을 구함              |

```mariadb
-- Finding the Maximum Value
SELECT MAX(a) FROM t1;

-- Finding the Minimum Value
SELECT MIN(a) FROM t1;

-- Finding the Average Value
SELECT AVG(a) FROM t1;
+--------+
| AVG(a) |
+--------+
| 2.0000 |
+--------+

-- Finding the Maximum Value and Grouping the Results
SELECT name, MAX(score) FROM student_tests GROUP BY name;
```

HAVING 절 (GROUP BY의 조건절)

```mariadb
/* 그룹열 중복제거, 그룹열에 해당하는 합칠열 값을 모두 더한값(SUM)이 1000보다 큰 행들을 출력 */
SELECT 그룹열, SUM(합칠열)
FROM 테이블명
GROUP BY 그룹열
HAVING SUM(합칠열) > 1000;

SELECT 그룹열, SUM(수량열*가격열)
FROM 테이블명
GROUP BY 그룹열
HAVING SUM(수량열*가격열) > 1000;
```



MariaDB SQL 쿼리 - INSERT, UPDATE, DELETE

INSERT 기본형식

```mariadb
USE DB명;
CREATE TABLE 테이블명 (열이름1 INT, 열이름2 CHAR(3), 열이름3 INT);

-- 위 테이블에 생성된 열이름 순서에 맞게 INSERT 됨
INSERT INTO 테이블명 VALUES (1, '김이박', 30); 
-- 열이름3에는 NULL값이 입력됨
INSERT INTO 테이블명(열이름1, 열이름2) VALUES (2, '박서왕'); 
-- 열이름 3,1,2 순서로 입력됨
INSERT INTO 테이블명(열이름3, 열이름1, 열이름2) VALUES (40, 3, '이전심'); 
```

AUTO_INCREMENT(순서열 같은 경우 자동으로 1부터 증가된 값을 입력해주는 기능)

```mariadb
USE DB명;
CREATE TABLE 테이블명 (
    순서열이름 INT AUTO_INCREMENT PRIMARY KEY, -- 순서열이름 열은 자동 증가, 기본키
    열이름2 CHAR(3),
    열이름3 INT
);
INSERT INTO 테이블명 VALUES (NULL, '곽민아', 35); -- NULL값이 아닌 1이 입력됨
INSERT INTO 테이블명 VALUES (NULL, '서나리', 32); -- 2가 입력됨
INSERT INTO 테이블명 VALUES (NULL, '유은성', 31); -- 3이 입력됨

CREATE TABLE student_details (
    id INT NOT NULL AUTO_INCREMENT, name CHAR(10),
    date_of_birth DATE, PRIMARY KEY (id)
);
INSERT INTO student_details (name, date_of_birth) VALUES 
 ('Chun', '1993-12-31'),
 ('Esben', '1946-01-01'),
 ('Kaolin', '1996-07-16'),
 ('Tatiana', '1988-04-13');
 
SELECT * FROM student_details;
+----+---------+---------------+
| id | name    | date_of_birth |
+----+---------+---------------+
|  1 | Chun    | 1993-12-31    |
|  2 | Esben   | 1946-01-01    |
|  3 | Kaolin  | 1996-07-16    |
|  4 | Tatiana | 1988-04-13    |
+----+---------+---------------+
```

다른 테이블의 조회값을 INSERT 하기

```mariadb
USE DB명;
CREATE TABLE 테이블명 ( 열이름1 INT, 열이름2 VARCHAR(50), 열이름3 VARCHAR(50) );
INSERT INTO 테이블명 
 SELECT 조회열1, 조회열2, 조회열3 -- 인서트할 테이블의 열과 같은 수, 같은 데이터형식을 SELECT 해야함
  FROM 조회DB.조회테이블;
```

UPDATE (데이터 수정)

```mariadb
USE DB명;
UPDATE 테이블명 SET 열이름 = 바꿀값 WHERE 조건문;

-- EX)
UPDATE 테이블명 SET 가격열 = 가격열 * 10; -- 가격열의 모든 값을 10을 곱하여 변경
```

조건부 데이터 입력(INSERT), 변경(UPDATE)

```mariadb
INSERT INTO 테이블명 VALUES ('값1', '값2', '값3') -- 값1,2,3을 테이블에 입력
	ON DUPLICATE KEY UPDATE 열이름2='값2', 열이름3='값3';
	/* 만약 기본키가 중복되면 열이름2를 값2로, 열이름3을 값3으로 업데이트 */
```

DELETE FORM (행단위 삭제)

```mariadb
USE DB명;
DELETE FROM 테이블명 WHERE 조건문;

-- EX)
DELETE FROM 테이블명 WHERE 이름열 = '김이박'; -- 이름열 값이 김이박인 행들 모두 삭제
```

DELETE, DROP, TRUNCATE 차이

```mariadb
DELETE FROM 테이블명;    -- DML: 트랜젝션 로그를 기록하여 느림. 테이블 모든 행 삭제
DROP TABLE 테이블명;     -- DDL: 트랜젝션 발생하지 않아 빠름. 테이블 자체를 삭제
TRUNCATE TABLE 테이블명; -- DDL: 트랜젝션 발생하지 않아 빠름. 테이블의 모든 행 삭제
```
