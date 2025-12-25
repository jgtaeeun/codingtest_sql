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





