### MariaDB SQL 쿼리 - 데이터 형식

> 숫자 데이터 형식

| 데이터 형식                        | 바이트 수 | 숫자 범위               | 설명                                                         |
| ---------------------------------- | --------- | ----------------------- | ------------------------------------------------------------ |
| BIT(N)                             | N/8       |                         | 1~64bit 표현. b'0000' 형식으로 표현                          |
| TINYINT                            | 1         | -128~127                | 정수                                                         |
| SMALLINT                           | 2         | -32,768~32,767          | 정수                                                         |
| MEDIUMINT                          | 3         | -8,338,608~8,388,607    | 정수                                                         |
| INT<br />INTEGER                   | 4         | 약 -21억~+21억          | 정수                                                         |
| BIGINT                             | 8         | 약 -900경~+900경        | 정수                                                         |
| FLOAT                              | 4         | -3.40E+38~-1.17E-38     | 소수점 아래 7자리까지 표현                                   |
| DOUBLE<br />REAL                   | 8         | -1.22E-308~1.79E-308    | 소수점 아래 15자리까지 표현                                  |
| DECIMAL(m,[d])<br />NUMERIC(m,[d]) | 5~17      | -10의 38승+1~10의38승-1 | 전체 자릿수(m)와 소숫점 이하 자릿수(d)를 가진 숫자형<br />예) DECIMAL(5, 2)는 전체 자릿수 5, 그 중 소수점 이하는 2자리 |

> 문자 데이터 형식

| 데이터 형식   | 바이트 수     | 설명                                                         |
| ------------- | ------------- | ------------------------------------------------------------ |
| CHAR(n)       | 1~255         | 고정길이 문자형. n을 1부터 255까지 지정. 그냥 CHAR만 쓰면 CHAR(1)과 동일한 의미 |
| VARCHAR(n)    | 1~65535       | 가변길이 문자형. n을 사용하면 1부터 65535까지 지정.          |
| BINARY(n)     | 1~255         | 고정길이의 이진 데이터 값                                    |
| VARBINARY(n)  | 1~255         | 가변길이의 이진 데이터 값                                    |
| TINYTEXT      | 1~255         | 255 크기의 TEXT 데이터 값                                    |
| TEXT          | 1~65535       | N 크기의 TEXT 데이터 값                                      |
| MEDIUMTEXT    | 1~16777215    | 16777215 크기의 데이터 값                                    |
| LONGTEXT      | 1~4294967295  | 최대 4GB 크기의 TEXT 데이터 값 (소셜 데이터 등을 저장)       |
| TINYBLOB      | 1~255         | 255 크기의 BLOB 데이터 값                                    |
| BLOB          | 1~65535       | N 크기의 BLOB 데이터 값                                      |
| MEDIUMBLOB    | 1~16777215    | 16777215 크기의 BLOB 데이터 값                               |
| LONGBLOB      | 1~4294967295  | 최대 4GB 크기의 BLOB 데이터 값 (동영상 데이터 등을 저장)     |
| ENUM(값들...) | 1 또는 2      | 최대 655535개의 열거형 데이터 값                             |
| SET(값들...)  | 1, 2, 3, 4, 8 | 최대 64개의 서로 다른 데이터 값                              |

> 날짜와 시간 데이터 형식

| 데이터 형식 | 바이트 수 | 설명                                                         |
| ----------- | --------- | ------------------------------------------------------------ |
| DATE        | 3         | 날짜는 1001-01-01 ~ 9999-12-31 까지 저장되며 날짜 형식만 사용 |
| TIME        | 3         | -838:59:59.000000 ~ 838:59:59.000000 까지 저장되며 'HH:MM:SS' 형식으로 사용됨 |
| DATETIME    | 8         | 날짜는 1001-01-01 00:00:00 ~ 9999-12-31 23:59:59 까지 저장되며 날짜 형식만 사용<br />'YYYY-MM-DD HH:MM:SS' 형식으로 사용됨 |
| TIMESTAMP   | 4         | 날짜는 1001-01-01 00:00:00 ~ 9999-12-31 23:59:59 까지 저장되며 날짜 형식만 사용<br />'YYYY-MM-DD HH:MM:SS' 형식으로 사용됨<br />time_zone 시스템 변수와 관련이 있으며 UTC 시간대로 변환하여 저장 |
| YEAR        | 1         | 1901~2155 까지 저장. 'YYYY' 형식으로 사용                    |

예시)

```mariadb
SELECT CAST ('2020-10-19 12:35:29.123' AS DATE) AS 'DATE';
SELECT CAST ('2020-10-19 12:35:29.123' AS TIME) AS 'TIME';
SELECT CAST ('2020-10-19 12:35:29.123' AS DATETIME) AS 'DATETIME';

/* 결과값 */
DATE		TIME	DATETIME
2022-10-19	12:3529	2022-10-19 12:35:29
```

> 기타 데이터 형식

| 데이터 형식 | 바이트 수 | 설명                                                         |
| ----------- | --------- | ------------------------------------------------------------ |
| GEOMETER    | N/A       | 공간 데이터 형식으로 선, 점 및 다각형 같은 공간 데이터 개체를 저장하고 조작 |
| JSON        | 8         | JSON (JavaScript Object Notation) 문서를 저장<br />(JSON 데이터 형식 MariaDB 10.2.7 부터 지원) |



MariaDB SQL 쿼리 - 변수 사용 및 데이터 형 변환

변수의 사용

```mariadb
SET @변수이름 = 변수의 값; -- 변수의 선언 및 값 대입
SELECT @변수이름; -- 변수의 값 출력

/* 사용 예제 */
USE DB명;

SET @변수명1 = 4;
SET @변수명2 = 2;
SET @변수명3 = 3.25; -- 실수 대입
SET @변수명4 = '지역명-->'; -- 문자 대입

SELECT @변수명1; -- 정수 출력
SELECT @변수명2 + @변수명3; -- 정수+실수 = 실수
SELECT @변수명4, 지역명열이름 FROM 테이블명; -- 1열은 '지역명-->', 2열은 테이블의 지역명 내용 출력
```

LIMIT에 변수 사용방법(기본적으로 변수 사용 불가능하나 PREPARE와 EXECUTE문을 활용해서 사용 가능)

```mariadb
SET @변수1 = 5;
PREPARE 쿼리이름
	FROM 'SELECT 열이름1, 열이름2 FROM 테이블명 ORDER BY 열이름1 LIMIT ?'; -- ?에 값이 입력됨
EXECUTE 쿼리이름 USING @변수1; -- 여기서 ?에 @변수1의 값을 대입
```

데이터 형 변환

```mariadb
-- 데이터 형 변환 방식1 CAST
CAST (expression AS 데이터형식 [(길이)])

-- 데이터 형 변환 방식2 CONVERT
CAST (expression, 데이터형식 [(길이)])

-- 형 변환 예시: 평균(AVG)가 소숫점으로 나오지 않도록 SIGNED INT로 형변환 출력
SELECT CAST(AVG(amount) AS SIGNED INTEGER) AS '평균 구매 개수' FROM 테이블명;
SELECT CONVERT(AVG(amount), SIGNED INTEGER) AS '평균 구매 개수' FROM 테이블명;

-- 형 변환 예시: 날짜 데이터 형식으로 변환하기
SELECT CAST('2020$12$12' AS DATE); -- 결과값: 2020-12-12
SELECT CAST('2022/12/12' AS DATE); -- 결과값: 2022-12-12
SELECT CAST('2022%12%12' AS DATE); -- 결과값: 2022-12-12
SELECT CAST('2022@12@12' AS DATE); -- 결과값: 2022-12-12

-- 암시적인 형 변환
SELECT '100' + '200'; -- 문자와 문자를 더함 (정수로 변환되서 연산됨)
SELECT CONCAT('100', '200'); -- 문자와 문자를 연결 (문자로 처리)
SELECT CONCAT(100, '200'); -- 정수와 문자를 연결 (정수가 문자로 변환되서 처리)
SELECT 1 > '2mega'; -- 정수 2로 변환되어서 비교
SELECT 3 > '2MEGA'; -- 정수 2로 변환되어서 비교
SELECT 0 = 'mega2'; -- 문자는 0으로 변환됨
```



MariaDB SQL 쿼리 - 내장 함수

> 제어 흐름 함수

```mariadb
/* IF(수식, 참, 거짓)
 * 수식이 참이면 2번째 값 출력, 거짓이면 3번째 값 출력 */
SELECT IF (100>200, '참', '거짓'); -- 거짓이 출력

/* IFNULL(수식1, 수식2) 
 * 수식1값이 NULL이면 수식2가 반환, NULL이 아니면 수식1이 반환 */
SELECT IFNULL(NULL, '널'), IFNULL(100, '널') -- 앞의 값은 NULL이라 '널', 뒤의 값은 100 반환
 
/* NULLIF(수식1, 수식2) 
 * 수식1과 수식2가 같으면 NULL을 반환, 다르면 수식1을 반환 */
SELECT NULLIF(100, 100), NULLIF(200, 100); -- 앞의 값은 같아서 NULL, 뒤의 값은 200 반환

/* CASE~WHEN~ELSE~END */
SELECT	CASE 10 -- CASE 값이 10일 때
		WHEN 1 THEN '일'
		WHEN 5 THEN '오'
		WHEN 10 THEN '십' -- WHEN 값이 10인것을 찾아 THEN 뒤에 값을 반환
		ELSE '모름' -- WHEN 값에 존재하지 않으면 ELSE 뒤에 값을 반환
	END;
```

> 문자열 함수

```mariadb
/* ASCII(아스키코드), CHAR(숫자) */
SELECT ASCII('A'), CHAR(65); -- 65 출력, 'A' 출력
```

```mariadb
/* BIT_LENGTH(문자열), CHAR_LENGTH(문자열), LENGTH(문자열)
BIT_할당된 Bit크기, 또는 문자 크기를 반환
CHAR_문자의 개수를 반환
LENGTH 할당된 바이트 수를 반환 */
SELECT BIT_LENGTH('abc'), CHAR_LENGTH('abc'), LENGTH('abc') -- 24, 3, 3(영문 3바이트)
SELECT BIT_LENGTH('가나다'), CHAR_LENGTH('가나다'), LENGTH('가나다') -- 72, 3, 9(한글 9바이트)
```

```mariadb
-- CONCAT(문자열1, 문자열2, ...), CONCAT_WS(문자열1, 문자열2, ...)
-- 문자열들을 이어준다. CONCAT_WS는 구분자와 함께 문자열을 이어준다.
SELECT CONCAT_WS('/', '2022', '01', '01'); -- 2020/01/01 출력
```

```mariadb
-- ELT(위치, 문자열1, 문자열2, ...), FIELD(찾을 문자열, 문자열1, 문자열2, ...), FIND_IN_SET(찾을 문자열, 문자열 리스트), INSTR(기준 문자열, 부분 문자열), LOCATE(부분 문자열, 기준 문자열)
SELECT ELT(2, '하나', '둘', '셋'); -- '둘' 출력
SELECT FIELD('둘', '하나', '둘', '셋'); -- 2 출력
SELECT FIND_IN_SET('둘', '하나,둘,셋'); -- 2 출력
SELECT INSTR('하나둘셋', '둘'); -- 3 출력
SELECT LOCATE('둘', '하나둘셋'); -- 3 출력 (LOCATE와 POSITION은 동일한 함수)
```

```mariadb
-- FORMAT(숫자, 소수점자리수)
SELECT FORMAT(123456.123456, 4); -- 123,456.1235 출력
```

```mariadb
-- BIN(숫자), HEX(숫자), OCT(숫자)
SELECT BIN(31), HEX(31), OCT(31); -- 2진수 11111, 16진수 1F, 8진수 37 출력
```

```mariadb
-- INSERT(기준 문자열, 위치, 길이, 삽입할 문자열)
-- 기준 문자열의 위치부터 길이만큼을 지우고, 삽입할 문자열을 끼워 넣음
SELECT INSERT('abcdefghi', 3, 4, '@@@@'), INSERT('abcdefghi', 3, 2, '@@@@'); -- ab@@@@ghi, ab@@@@efghi 출력
```

```mariadb
-- LEFT(문자열, 길이), RIGHT(문자열, 길이)
SELECT LEFT('abcdefghi', 3), RIGHT('abcdefghi', 3); -- abc, ghi 출력
```

```mariadb
-- UPPER(문자열), LOWER(문자열)
SELECT LOWER('abcdEFGH'), UPPER('abcdEFGH'); -- abcdefgh, ABCDEFGH를 출력
-- LOWER()와 LCASE()는 동일함수. UPPER()와 UCASE()는 동일함수
```

```mariadb
-- LPAD(문자열, 길이, 채울 문자열), RPAD(문자열, 길이, 채울 문자열)
-- 문자열을 길이만큼 늘린 후에 빈 곳을 채울 문자열로 채움
SELECT LPAD('김이박', 5, '##'), RPAD('김이박', 5, '##'); -- '##김이박', '김이박##' 출력
```

```mariadb
-- LTRIM(문자열), RTRIM(문자열)
-- 문자열 왼쪽, 오른쪽 공백을 제거. 가운데 공백은 제거하지 않음
SELECT LTRIM('  김이박'), RTRIM('김이박  '); -- '김이박', '김이박' 출력
```

```mariadb
-- TRIM(문자열), TRIM(방향 자를 문자열 FROM 문자열)
-- TRIM() 앞 뒤 공백을 모두 제거함. 방향: LEADING 앞쪽, BOTH 양쪽, TRAILING 뒤쪽
SELECT TRIM('  김이박  '); -- '김이박' 출력
SELECT TRIM(BOTH 'G' FROM 'GGG김이박GGG'); -- '김이박' 출력
```

```mariadb
-- REPEAT(문자열, 횟수)
SELECT REPEAT('김이박', 3); -- '김이박김이박김이박' 출력
```

```mariadb
-- REPLACE(문자열, 원래 문자열, 바꿀 문자열)
SELECT REPLACE('이제는 잘 수 있겠다', '있겠다', '없겠다'); -- '이제는 잘 수 없겠다'로 변경 출력
```

```mariadb
-- REVERSE(문자열)
SELECT REVERSE('김이박'); -- '박이김' 출력
```

```mariadb
-- SPACE(길이)
SELECT CONCAT('이제는', SPACE(10), '졸립다'); -- '이제는    졸립다' 출력
```

```mariadb
-- SUBSTRING(문자열, 시작위치, 길이) or (문자열 FROM 시작위치 FOR 길이)
-- 시작위치부터 길이만큼 문자를 반환. 길이가 생략되면 준마열 끝까지 반환
SELECT SUBSTRING('이제그만자자', 3, 2); -- '그만' 출력
-- SUBSTRING(), SUBSTR(), MID()는 모두 같은 함수
```

```mariadb
-- SUBSTRING_INDEX(문자열, 구분자, 횟수)
-- 문자열에서 구분자의 횟수번째 나오는 부분 뒤를 버림. 횟수가 음수면 우측부터 횟수번째 나오는 앞을 버림
SELECT SUBSTRING_INDEX('cafe.daum.net', '.', '2'); -- cafe.daum 출력
SELECT SUBSTRING_INDEX('cafe.daum.net', '.', '-2'); -- daum.net 출력
```

> 수학 함수

```mariadb
-- ABS(숫자)
SELECT ABS(-100); -- 절대값 100이 출력
```

```mariadb
-- CEILING(숫자), FLOOR(숫자), ROUND(숫자)
SELECT CEILING(4.7), FLOOR(4.7), ROUND(4.7); -- 올림 5, 내림 4, 반올림 5 출력
```

```mariadb
-- CONV(숫자, 원래진수, 변환할 진수)
SELECT CONV('AA', 16, 2); -- 16진수 AA를 2진수로 변환하여 10101010 출력
SELECT CONV(100, 10, 8); -- 10진수 100을 8진수로 변환하여 144 출력
```

```mariadb
-- DEGREES(숫자), RADIANS(숫자), PI()
-- 라디안 값을 각도값으로 변환, 각도값을 라디안으로 변환, PI값인 3.141592를 반환
SELECT DEGREES(PI()), RADIANS(180); -- 180, 3.141592 출력
```

```mariadb
-- MOD(숫자1, 숫자2) or 숫자1 % 숫자2 or 숫자1 MOD 숫자2
-- 숫자1을 숫자2로 나눈 나머지 값을 반환
SELECT MOD(157, 10), 157 % 10, 157 MOD 10; -- 모두 157을 10으로 나눈 나머지값인 7을 반환
```

```mariadb
-- POW(숫자1, 숫자2), SQRT(숫자)
-- 숫자1의 숫자2 만큼 제곱값 반환, 숫자의 제곱근을 반환
SELECT POW(2, 3), SQRT(9); -- 8, 3 출력
```

```mariadb
-- RAND()
-- 0이상 1미만의 실수를 반환. 실행할 때마다 값이 변경됨
SELECT RAND(), FLOOR(1 + (RAND() * (7-1))); -- 0 ~ 1 실수 반환. 1 ~ 6 정수 반환
```

```mariadb
-- SIGN(숫자)
-- 숫자가 양수, 0, 음수 인지를 구함. 각 1, 0, -1을 반환.
SELECT SIGN(100), SIGN(0), SIGN(-100.123); -- 1, 0, -1 출력
```

```mariadb
-- TRUNCATE(숫자, 정수)
-- 숫자를 소수점을 기준으로 정수 위치(양수는 우측, 음수는 좌측)까지 구하고 나머지는 버린다
SELECT TRUNCATE(12345.12345, 2), TRUNCATE(12345.12345, -2); -- 12345.123, 12300 반환
```

날짜 및 시간 함수

```mariadb
-- ADDDATE(날짜, 차이), SUBDATE(날짜, 차이)
SELECT ADDDATE('2022-01-01', INTERVAL 31 DAY), ADDDATE('2022-01-01', INTERVAL 1 MONTH);
-- 2022-02-01 출력
SELECT SUBDATE('2022-01-01', INTERVAL 31 DAY), SUBDATE('2022-01-01', INTERVAL 1 MONTH);
-- 2021-12-01 출력
```

```mariadb
-- ADDTIME(날짜/시간, 시간), SUBTIME(날짜/시간, 시간)
-- 날짜/시간을 기준으로 시간을 더한 결과, 뺀 결과를 반환
SELECT ADDTIME('2022-01-01 23:59:59', '1:1:1'), ADDTIME('15:00:00', '2:10:10');
-- 2022-01-02 01:01:00, 17:10:10 출력
SELECT SUBTIME('2022-01-01 23:59:59', '1:1:1'), SUBTIME('15:00:00', '2:10:10');
-- 2022-01-01 22:58:58, 12:49:50 출력
```

```mariadb
CURDATE() 현재 년-월-일, CURTIME() 현재 시:분:초, NOW()와 SYSDATE() 현재 년-월-일 시:분:초 출력

YEAR(날짜), MONTH(날짜), DAY(날짜), HOUR(시간), MINUTE(시간), SECOND(시간), MICROSECOND(시간) -- (현재 날짜 또는 시간에서 년, 월, 일, 시, 분, 초, 밀리초를 출력)
```

```mariadb
-- DATE(), TIME()
SELECT DATE(NOW()), TIME(NOW()) -- 현재 날짜, 시간 출력
```

```mariadb
-- DATEDIFF(날짜, 날짜2), TIMEDIFF(날짜1or시간1, 날짜2or시간2)
-- 날짜1 - 날짜2 일수 결과 반환, 시간1 - 시간2 시간 결과 반환
SELECT DATEDIFF('2022-01-01', NOW()), TIMEDIFF('23:23:59', '12:11:10');
```

```mariadb
-- DAYOFWEEK(날짜), MONTHNAME(), DAYOFYEAR(날짜)
-- 현재 요일(순서), 월이름, 일년 중 몇일이 지났는지를 반환
SELECT DAYOFWEEK(CURDATE()), MONTHNAME(CURDATE()), DAYOFYEAR(CURDATE());
```

```mariadb
-- LAST_DAY(날짜)
-- 날짜 달의 마지막 날짜를 반환. 보통 날짜 달의 마지막 날짜를 확인할 때 사용
SELECT LAST_DAY('2022-02-01'); -- 2022-02-28 출력
```

```mariadb
-- MAKEDATE(연도, 정수)
-- 연도에서 정수만큼 지난 날짜를 구한다
SELECT MAKEDATE(2022, 32); -- 2022-02-01 출력
```

```mariadb
-- MAKETIME(시, 분, 초)
SELECT MAKETIME(12, 11, 10) -- 12:11:10 출력
```

```mariadb
-- PERIOD_ADD(년월, 개월수), PERIOD_DIFF(년월1, 년월2)
-- 년월(YYYYMM)에서 개월수만큼 지난 년월을 구한다. 년월1 - 년월2의 개월수를 구한다.
SELECT PERIOD_ADD(202201, 11), PERIOD_DIFF(202201, 201812); -- 202212, 37 출력
```

```mariadb
-- QUARTER(날짜)
-- 몇분기인지 출력
SELECT QUARTER('2022-07-07'); -- 3 출력
```

```mariadb
-- TIME_TO_SEC(시간)
-- 시간을 초 단위로 구한다
SELECT TIME_TO_SEC('12:11:10'); -- 43870 출력
```

시스템 정보 함수

```mariadb
-- USER(), DATABASE()
SELECT CURRENT_USER(), DATABASE() -- 현재 사용자, 현재 선택된 DATABASE 출력
```

```mariadb
-- FOUND_ROWS()
USE DB명; -- DB선택
SELECT * FROM 테이블명;
SELECT FOUND_ROWS(); -- 바로 앞의 SELECT에서 조회된 행의 개수를 구함
```

```mariadb
-- ROW_COUNT()
USE DB명;
UPDATE 테이블명 SET 값이숫자인열이름=값이숫자인열이름*2;
SELECT ROW_COUNT(); -- 바로 앞의 INSERT, UPDATE, DELETE문에서 변경된 행들의 개수를 반환. CREATE, DROP문은 0을 반환. SELECT는 -1을 반환
```

```mariadb
-- VERSION() : 현재 DBMS의 버전을 확인
-- SLEEP(초)
SELECT SLEEP(5);
SELECT '5초 후에 보여요'; -- 5초 후 '5초 후에 보여요' 출력
```
