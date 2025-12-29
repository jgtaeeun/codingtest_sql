# codingtest_sql

## 1일차(12/23)

|유형|문제|코드|
|:--:|:--:|:--:|
|select|평균 일일 대여 요금 구하기 <br> https://school.programmers.co.kr/learn/courses/30/lessons/151136| ROUND(AVG(DAILY_FEE),0) 은 소수첫째자리에서 반올림 |

```sql
SELECT ROUND(AVG(DAILY_FEE),0) AS AVERAGE_FEE
FROM bookrentalshop.car_rental_company_car
where CAR_TYPE = 'SUV';
```

|유형|문제|코드|
|:--:|:--:|:--:|
|select <br> (+join)|조건에 부합하는 중고거래 댓글 조회하기 <br> https://school.programmers.co.kr/learn/courses/30/lessons/164673| NOW(): 현재 날짜와 시간을 모두 반환 (2022-10-31 14:30:59) <br>DATE_ADD(NOW(), INTERVAL 7 DAY) (7일 후)<br>DATE_FORMAT(CREATED_DATE, '%Y/%m/%d') |

```sql
-- 주의점: 만약 CREATED_DATE 데이터 타입이 DATETIME이라면 주의해야 합니다. '2022-10-31'은 내부적으로 '2022-10-31 00:00:00'으로 인식됩니다. 따라서 10월 31일 오후 2시에 작성된 글은 누락될 위험이 있습니다.

SELECT TITLE,a.BOARD_ID as BOARD_ID ,REPLY_ID,b.WRITER_ID as WRITER_ID, b.CONTENTS as CONTENTS, b.CREATED_DATE as CREATED_DATE
from USED_GOODS_BOARD as a
join USED_GOODS_REPLY as b
on a.BOARD_ID = b.BOARD_ID
where a.CREATED_DATE	between '2022-10-01' and '2022-10-31'
order by b.CREATED_DATE , TITLE
```

```sql
-- 해결책: BETWEEN '2022-10-01 00:00:00' AND '2022-10-31 23:59:59'로 쓰거나, 부등호를 사용하여 a.CREATED_DATE >= '2022-10-01' AND a.CREATED_DATE < '2022-11-01'로 작성하는 것이 가장 안전합니다.

SELECT TITLE,a.BOARD_ID as BOARD_ID ,REPLY_ID,b.WRITER_ID as WRITER_ID, b.CONTENTS as CONTENTS, b.CREATED_DATE as CREATED_DATE
from USED_GOODS_BOARD as a
join USED_GOODS_REPLY as b
on a.BOARD_ID = b.BOARD_ID
where a.CREATED_DATE	>= '2022-10-01' and a.CREATED_DATE <'2022-11-01'
order by b.CREATED_DATE , TITLE
```

  |함수|설명|
  |:--:|:--:|
  |NOW()|현재 날짜와 시간을 모두 반환|
  |CURDATE()|현재 날짜만|
  |CURTIME()|현재 시간만|
  |YEAR(date), MONTH(date), DAY(date)|연도, 월, 일|
  |HOUR(time), MINUTE(time), SECOND(time)|시, 분, 초|
  |WEEKDAY(date)|요일 인덱스 (0: 월요일 ~ 6: 일요일)|
  |DATE_ADD(NOW(), INTERVAL 7 DAY) ,DATE_SUB(NOW(), INTERVAL 1 MONTH)  |7일 후, 한 달 전|
  |DATEDIFF(날짜1, 날짜2), TIMEDIFF(시간1, 시간2)|날짜1 - 날짜2, 시간1-시간2|
  |DATE_FORMAT(CREATED_DATE, '%Y/%m/%d')|'2022/10/01'로 변환|
  
  |기호|예시|
  |:--:|:--:|
  |%Y|2022|
  |%y|22|
  |%m|01~12|
  |%d|01~31|
  |%H|00~23|
  |%i|00~59|
  |%s|00~59|
  |%W|요일 이름 (영어)|


## 2일차(12/25)

|유형|문제|코드|
|:--:|:--:|:--:|
|select|조건에 맞는 도서 리스트 출력하기 <br> https://school.programmers.co.kr/learn/courses/30/lessons/144853|DATE_FORMAT(CREATED_DATE, '%Y-%m-%d') |

```sql
SELECT BOOK_ID , DATE_FORMAT(PUBLISHED_DATE , '%Y-%m-%d') as PUBLISHED_DATE
FROM BOOK
WHERE CATEGORY = '인문' AND YEAR(PUBLISHED_DATE) = 2021
ORDER BY PUBLISHED_DATE 
```

|유형|문제|코드|
|:--:|:--:|:--:|
|select|12세 이하인 여자 환자 목록 출력하기<br> https://school.programmers.co.kr/learn/courses/30/lessons/132201| IFNULL(필드명, NULL일 때 값) <br>COALESCE(필드명, NULL일 때 값) |

```sql
SELECT PT_NAME, PT_NO,  GEND_CD, AGE, IFNULL( TLNO , 'NONE' ) AS TLNO
from PATIENT
WHERE AGE<=12 AND GEND_CD='W'
ORDER BY AGE DESC , PT_NAME
```
   |함수|설명|
   |:--:|:--:|
   |IFNULL(필드명, NULL일 때 값)|MySQL/MariaDB 전용|
   |COALESCE(필드명, NULL일 때 값)|MySQL, PostgreSQL, Oracle, SQL Server 등|
   |CASE WHEN 필드명 IS NULL THEN NULL일 때 값 ELSE 필드명 END|복잡한 조건 제어 가능|
   |NVL(필드명, NULL일 때 값) |Oracle 전용|




## 3일차(12/29)

|유형|문제|코드|
|:--:|:--:|:--:|
|select|흉부외과 또는 일반외과 의사 목록 출력하기 <br>https://school.programmers.co.kr/learn/courses/30/lessons/132203|WHERE MCDP_CD = 'CS' OR MCDP_CD = 'GS'  <br>WHERE MCDP_CD IN ( 'CS' ,'GS') |

```sql
SELECT DR_NAME, DR_ID, , MCDP_CD, DATE_FORMAT( HIRE_YMD , '%Y-%m-%d') AS HIRE_YMD 
FROM DOCTOR 
WHERE MCDP_CD = 'CS' OR MCDP_CD = 'GS'
ORDER BY HIRE_YMD  DESC, DR_NAME ASC 
```

|유형|문제|코드|
|:--:|:--:|:--:|
|select|3월에 태어난 여성 회원 목록 출력하기 <br>https://school.programmers.co.kr/learn/courses/30/lessons/131120|Like '%-01-% '<br>  MySQL 같은 DB 엔진은 날짜 형식(DATE, DATETIME)의 데이터를 조회할 때 내부적으로 문자열로 자동 형변환(Implicit Casting)을 지원|

```sql
SELECT MEMBER_ID, MEMBER_NAME,GENDER,DATE_FORMAT(DATE_OF_BIRTH ,'%Y-%m-%d') AS DATE_OF_BIRTH	
FROM MEMBER_PROFILE
WHERE MONTH(DATE_OF_BIRTH) = 3 AND GENDER= 'W' AND TLNO IS NOT NULL
ORDER BY MEMBER_ID ASC 
```

- 날짜 필터링
  - 간단한 확인용: MONTH() 함수가 가장 직관적이고 편합니다.
  - 성능 최적화 필요 시: **BETWEEN이나 대소 비교(>=, <)를 사용하는 것이 가장 좋습니다.**
  - LIKE 사용 시: 날짜 포맷(YYYY-MM-DD)을 고려하여 엉뚱한 날짜(일자가 01일인 경우 등)가 걸리지 않게 조심해야 합니다.


- null값 조건인가/처리인가

  |구분|IS NOT NULL|IFNULL()|
  |:--:|:--:|:--:|
  |성격비교| 	연산자 (조건문)|		함수 (값 처리)|
  |주요 위치|	WHERE 절	|		SELECT 절, 연산식 내부|
  |하는 일|	NULL이 아닌 행을 찾아냄|	NULL을 다른 값으로 채워 넣음|
  |리턴값|	참/거짓 (Boolean)	|	실제 데이터 값 (문자, 숫자 등)|


|유형|문제|코드|
|:--:|:--:|:--:|
|select|강원도에 위치한 생산공장 목록 출력하기 <br>https://school.programmers.co.kr/learn/courses/30/lessons/131112|LIKE '강원도%'  <br> <br> % (퍼센트) : 0개 이상의 모든 문자 <br> _ (언더바) : 정확히 1개의 문자 (자릿수 고정)|

```sql
SELECT FACTORY_ID,FACTORY_NAME,ADDRESS
FROM FOOD_FACTORY
WHERE ADDRESS LIKE '강원도%'
ORDER BY 1
```

- LIKE 사용 시 주의사항 (성능과 정확도)
  - 인덱스(Index) 성능 저하: LIKE '김%'처럼 앞부분이 고정된 경우(Prefix)는 인덱스를 탈 수 있어 빠릅니다. 하지만 LIKE '%김'이나 LIKE '%김%'처럼 앞에 %가 붙으면 DB가 모든 데이터를 다 뒤져야 하므로(Full Scan) 매우 느려집니다. 데이터가 많을 때는 주의해야 합니다.
  - 대소문자 구분: MySQL은 기본적으로 설정(Collation)에 따라 대소문자를 구분하지 않는 경우가 많지만, Oracle이나 PostgreSQL은 구분하는 경우가 많으므로 확인이 필요합니다.
  - 특수문자 검색 (ESCAPE): 만약 실제 데이터 안에 %나 _가 포함되어 있어 이를 검색하고 싶다면 ESCAPE 문을 사용해야 합니다.
  ```sql
  -- idx/Author/Names :  60/	봄비와씨앗	/100% 순서대로 말하기 도서	
  -- 책제목이 100%라는 문자열로 시작되는 것
  SELECT * FROM bookrentalshop.bookstbl	WHERE Names like '100#%%' escape '#'  ;
  ```
