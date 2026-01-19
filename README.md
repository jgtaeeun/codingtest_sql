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

## 4일차(12/30)

|유형|문제|코드|
|:--:|:--:|:--:|
|select|서울에 위치한 식당 목록 출력하기<br>https://school.programmers.co.kr/learn/courses/30/lessons/131118|SQL에서 별칭(AS)<br>1. 연산이 들어간 컬럼 (계산식)<br>2. 컬럼 이름을 바꿔야 할 때|

```sql
SELECT 
    I.REST_ID, 
    I.REST_NAME, 
    I.FOOD_TYPE, 
    I.FAVORITES, 
    I.ADDRESS, 
    ROUND(AVG(R.REVIEW_SCORE), 2) AS SCORE
FROM REST_INFO I
JOIN REST_REVIEW R
ON I.REST_ID = R.REST_ID
WHERE I.ADDRESS LIKE '서울%'
GROUP BY I.REST_ID,I.REST_NAME,I.FOOD_TYPE, I.FAVORITES, I.ADDRESS
ORDER BY SCORE DESC, I.FAVORITES DESC;
```

|코드 수정사항|설명|
|:--:|:--:|
|SELECT I.REST_ID	|	I.REST_ID	결과 컬럼명은 어차피 REST_ID로 나오므로 별칭 생략 가능|
|GROUP BY I.REST_ID, I.REST_NAME |컬럼 명확성<br>여러 테이블 조인 시, 어느 테이블 컬럼인지 명시하는 것이 유지보수에 좋음|

|유형|문제|코드|
|:--:|:--:|:--:|
|select|재구매가 일어난 상품과 회원 리스트 구하기<br>https://school.programmers.co.kr/learn/courses/30/lessons/131536|COUNT(*): 테이블의 전체 행(Row) 개수. 특정 컬럼에 NULL이 들어있어도 행 자체가 존재하면 개수에 포함합니다.<br> <br>COUNT(컬럼명): 해당 컬럼의 값이 NULL이 아닌 것만 골라서 개수를 셉니다.|

```sql
SELECT USER_ID, PRODUCT_ID
From ONLINE_SALE 
Group by USER_ID, PRODUCT_ID
having count(*) >1
order by 1 asc, 2 desc
```

- 보통 실무에서는 다음과 같은 이유로 본인이 작성하신 COUNT(*)를 더 자주 사용합니다.
    - 가독성: "그룹 내 데이터의 개수"를 세는 것이 목적이라면 COUNT(*)가 더 직관적입니다.
    - 성능 최적화: 최신 데이터베이스(DB) 엔진은 COUNT(*)를 처리할 때 가장 효율적인 인덱스를 알아서 찾아 사용하도록 최적화되어 있습니다.
    - 안전성: 만약 나중에 다른 컬럼으로 그룹핑을 변경하더라도 쿼리 구조를 크게 고칠 필요가 없습니다.

|유형|문제|코드|
|:--:|:--:|:--:|
|select|모든 레코드 조회하기<br>https://school.programmers.co.kr/learn/courses/30/lessons/59034|SELECT *|

```sql
SELECT ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE
from ANIMAL_INS 
order by 1 asc
```
```sql
Select *
from ANIMAL_INS 
order by ANIMAL_ID
```

## 5일차(1/3)

|유형|문제|코드|
|:--:|:--:|:--:|
|select|오프라인/온라인 판매 데이터 통합하기<br>https://school.programmers.co.kr/learn/courses/30/lessons/131537|Union All|

```sql
SELECT DATE_FORMAT(SALES_DATE, '%Y-%m-%d') AS SALES_DATE, 
       PRODUCT_ID, 
       USER_ID, 
       SALES_AMOUNT
FROM (
    -- 온라인 판매 데이터
    SELECT SALES_DATE, PRODUCT_ID, USER_ID, SALES_AMOUNT
    FROM ONLINE_SALE
    WHERE SALES_DATE >= '2022-03-01' and SALES_DATE <'2022-04-01'
    UNION ALL

    -- 오프라인 판매 데이터 (USER_ID는 NULL로 처리)
    SELECT SALES_DATE, PRODUCT_ID, NULL AS USER_ID, SALES_AMOUNT
    FROM OFFLINE_SALE
    WHERE SALES_DATE LIKE '2022-03%'
) AS COMBINED_SALES
ORDER BY SALES_DATE ASC, PRODUCT_ID ASC, USER_ID ASC;
```
- UNION은 중복된 행을 제거하지만, UNION ALL은 모든 데이터를 그대로 합칩니다.
- 이 문제에서는 판매 데이터의 중복 제거가 필요 없으므로 성능이 더 빠른 UNION ALL을 사용하는 것이 관례입니다
- UNION의 3대 필수 규칙규칙상세 내용
    - 컬럼 개수 동일모든 SELECT 문에 나열된 컬럼의 개수가 반드시 같아야 합니다
    - 데이터 타입 호환대응하는 위치의 컬럼들은 데이터 타입이 같거나, 최소한 암시적 변환이 가능할 정도로 호환(Compatible)되어야 합니다.
    - 컬럼 순서 일치데이터의 의미에 맞게 컬럼의 순서가 동일해야 합니다.

## 6일차(1/5)

|유형|문제|코드|
|:--:|:--:|:--:|
|select|어린 동물 찾기<br>https://school.programmers.co.kr/learn/courses/30/lessons/59037|SQL 논리 연산자 <br> = 같다<br> != 또는 <> 같지 않다 |

```sql
SELECT ANIMAL_ID ,NAME
from ANIMAL_INS
where not INTAKE_CONDITION = 'Aged'
order by ANIMAL_ID;
```

```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION != 'Aged'
ORDER BY ANIMAL_ID;
```


|유형|문제|코드|
|:--:|:--:|:--:|
|select|상위 n개 레코드<br>https://school.programmers.co.kr/learn/courses/30/lessons/59405|LIMIT|

```sql
SELECT NAME 
from ANIMAL_INS
order by DATETIME limit 1
```

```sql
-- Oracle 방식
SELECT NAME
FROM (SELECT * FROM ANIMAL_INS ORDER BY DATETIME)
WHERE ROWNUM <= 1;
```
- 오라클 ROWNUM의 핵심 특징
  - ROWNUM은 1번부터만 가져올 수 있다. (ROWNUM <= n 형태)
  - 정렬이 먼저 되어야 한다면, ORDER BY를 안쪽(서브쿼리)에 넣어야 한다.

|유형|문제|코드|
|:--:|:--:|:--:|
|select|조건에 맞는 회원수 구하기<br>https://school.programmers.co.kr/learn/courses/30/lessons/131535|COUNT(*) <br> YEAR() <br> between |

```sql
SELECT count(*) as USERS
from USER_INFO
where JOINED >= '2021-01-01' and JOINED < '2022-01-01'
and  AGE between 20 and 29;
```

|유형|문제|코드|
|:--:|:--:|:--:|
|select|업그레이드 된 아이템 구하기<br>https://school.programmers.co.kr/learn/courses/30/lessons/273711|서브쿼리를 활용한 IN 방식|

```sql
select ITEM_ID,	ITEM_NAME,	RARITY
from ITEM_INFO
where ITEM_ID in ( SELECT ITEM_TREE.ITEM_ID FROM ITEM_TREE JOIN (select ITEM_ID from ITEM_INFO WHERE RARITY = 'RARE') as  temp on ITEM_TREE.PARENT_ITEM_ID = temp.ITEM_ID)
order by ITEM_ID desc
```

```sql
SELECT ITEM_ID, ITEM_NAME, RARITY
FROM ITEM_INFO
WHERE ITEM_ID IN (
    SELECT T.ITEM_ID
    FROM ITEM_TREE T
    JOIN ITEM_INFO I ON T.PARENT_ITEM_ID = I.ITEM_ID
    WHERE I.RARITY = 'RARE'
)
ORDER BY ITEM_ID DESC;
```



- 트리 테이블(ITEM_TREE)의 컬럼 해석
  - PARENT_ITEM_ID: 업그레이드 전 (뿌리, 재료)
  - ITEM_ID: 업그레이드 후 (결과물)
  - 문제에서 "업그레이드 된 아이템의 정보"를 원했으므로, 최종적으로 우리는 ITEM_TREE.ITEM_ID에 해당하는 상세 정보를 찾아야 합니다.
- 연결 고리로서의 JOIN 활용
  - 1단계: ITEM_INFO에서 'RARE' 아이템들의 ID를 찾는다.
  - 2단계: 위에서 찾은 ID를 ITEM_TREE의 PARENT_ITEM_ID와 매칭시킨다.
  - 3단계: 매칭된 행의 ITEM_ID를 확인한다.
  - 4단계: 그 ITEM_ID의 진짜 이름과 등급을 알기 위해 다시 ITEM_INFO를 조회한다.

## 7일차(1/6)

|유형|문제|코드|
|:--:|:--:|:--:|
|select|Python 개발자 찾기<br>https://school.programmers.co.kr/learn/courses/30/lessons/276013|OR|

```sql
SELECT ID, EMAIL,FIRST_NAME, LAST_NAME
FROM DEVELOPER_INFOS
WHERE SKILL_1 ='Python' OR  SKILL_2 ='Python' OR  SKILL_3 ='Python'
ORDER BY ID
```

|유형|문제|코드|
|:--:|:--:|:--:|
|select|조건에 맞는 개발자 찾기<br>https://school.programmers.co.kr/learn/courses/30/lessons/276034|비트 연산자 &|

```sql
SELECT ID, EMAIL, FIRST_NAME, LAST_NAME
FROM DEVELOPERS
WHERE 
    -- Python 코드를 가진 비트가 1인지 확인
    SKILL_CODE & (SELECT CODE FROM SKILLCODES WHERE NAME = 'Python')
    OR 
    -- C# 코드를 가진 비트가 1인지 확인
    SKILL_CODE & (SELECT CODE FROM SKILLCODES WHERE NAME = 'C#')
ORDER BY ID;
```

- 비트 연산자 & (AND)를 사용하여 특정 기술을 가졌는지 확인하는 원리
  - 각 기술의 CODE는 $2^n$ 형태로 되어 있습니다. 이것을 2진수로 바꾸면 단 한 곳만 1이고 나머지는 모두 0인 형태가 됩니다.
    - Python (256): 0001 0000 0000
    - C# (1024): 0100 0000 0000
  - 개발자의 SKILL_CODE는 자신이 가진 기술 코드들의 합입니다. 만약 어떤 개발자가 Python과 다른 기술(예: 16)을 가지고 있다면, 그 사람의 코드는 256 + 16 = 272가 됩니다.
    - 개발자 A (272): 0001 0001 0000 (2진수)
  - & (AND) 연산은 두 숫자를 비교해서 둘 다 1인 자리만 1로 남깁니다.
    - Case A: Python을 가진 개발자의 경우개발자 A(272)와 Python(256)을 & 연산해 보겠습니다.
      - 0001 0001 0000 (개발자 A: 272) & 0001 0000 0000 (Python: 256) 
      - 0001 0000 0000 (결과: 256)
      - 결과가 0이 아님 : 개발자가 Python 비트를 가지고 있다는 뜻입니다
    - Case B: Python이 없는 개발자의 경우개발자 B(1040, C#과 다른 기술 보유)와 Python(256)을 & 연산해 보겠습니다.
      - 0100 0001 0000 (개발자 B: 1040) & 0001 0000 0000 (Python: 256)
      - 0000 0000 0000 (결과: 0)
      - 결과가 0임: 개발자의 비트 중 Python 자리에 1이 없다는 뜻입니다.
  - 따라서 SQL에서 WHERE SKILL_CODE & 256 연산을 수행
    - 결과값이 0이 아니면 (비트가 겹치면) → TRUE (데이터 추출)
    - 결과값이 0이면 (비트가 안 겹치면) → FALSE (제외)

- 왜 BIN()을 안 써도 되나요?
  - 컴퓨터는 INTEGER(정수) 타입의 데이터를 저장할 때 이미 내부적으로 이진수(Binary) 상태로 저장하고 있습니다.
  - 우리가 SQL에 12라고 써도, 컴퓨터는 그것을 1100으로 인식합니다.
  - & (비트 연산자)는 숫자 데이터가 들어오면 자동으로 이진수 수준에서 비트를 비교합니다.
  - BIN()은 숫자를 '눈으로 보기 편한 문자열'로 바꾸는 함수일 뿐이며, 연산에는 필요하지 않습니다.


- 비트 연산자
  
|연산자|설명|	예시|	결과 (2진수)|
|:--:|:--:|:--:|:--:|
|&|	비트 AND	|SELECT 12 & 5;|	4 (1100 & 0101 = 0100)|
|\|	|비트 OR	|SELECT 12 \| 5;|	13 (1100 \| 0101 = 1101)|
|^|	비트 XOR	|SELECT 12 ^ 5;|	9 (1100 ^ 0101 = 1001)|
|~|	비트 NOT|	SELECT ~1;|	64비트 최대값 ($2^{64}-1$) -1|
|<<|	왼쪽 시프트	|SELECT 1 << 2;	|4 (0001 → 0100)|
|>>|	오른쪽 시프트|	SELECT 4 >> 2;|	1 (0100 → 0001)|


- ~1이 왜 그 값이 되는가?
  - $2^1$ (2): 10 → 여기서 1을 빼면 01 (모든 비트가 1)
  - $2^2$ (4): 100 → 여기서 1을 빼면 011 (모든 비트가 1)
  - $2^3$ (8)- 1 은  0111 (모든 비트가 1)
  - 즉, $2^n - 1$은 "n개의 비트가 모두 1로 채워진 상태"를 의미합니다
  - 64비트 최대값 ($2^{64}-1$):1111...1111 (1이 64개)
  - 우리가 구한 ~1의 결과:1111...1110 (1이 63개이고 끝에만 0)
  - 산술적으로 표현하면 $2^{64} - 2$
  
## 8일차(1/15)

|유형|문제|코드|
|:--:|:--:|:--:|
|select|가장 큰 물고기 10마리 구하기<br>https://school.programmers.co.kr/learn/courses/30/lessons/298517| LIMIT 구문은 항상 쿼리의 가장 마지막에 위치해야 합니다.|

```sql
SELECT ID, LENGTH
FROM FISH_INFO
ORDER BY LENGTH DESC, ID ASC
LIMIT 10;
```
- LIMIT 구문은 항상 쿼리의 가장 마지막에 위치해야 합니다.

- NULL 처리
  - 이 문제의 데이터셋에서는 LENGTH가 WHERE LENGTH > 10 같은 조건이 필요할 수 있지만, 상위 10마리를 뽑는 DESC 정렬에서는 NULL이 자동으로 하단으로 밀려나기 때문에 별도의 IS NOT NULL 없이도 정답 처리가 되는 경우가 많습니다.


## 9일차(1/19)

|유형|문제|코드|
|:--:|:--:|:--:|
|select|대장균들의 자식의 수 구하기<br>https://school.programmers.co.kr/learn/courses/30/lessons/299305| INNER JOIN (교집합) <br> 양쪽 테이블에 모두 존재하는 데이터만 합치는 방식입니다. <br>  <br>  LEFT (OUTER) JOIN <br> 왼쪽 테이블의 모든 데이터를 가져오고, 오른쪽 테이블에서 일치하는 데이터를 붙입니다.|

```sql
SELECT E.ID, COUNT(T.PARENT_ID) AS CHILD_COUNT
FROM ECOLI_DATA  E
LEFT JOIN  ECOLI_DATA T
ON E. ID= T.PARENT_ID
GROUP BY E.ID
ORDER BY E.ID ASC
```
```sql
-- 스칼라 서브쿼리를 이용한 풀이
SELECT 
    ID, 
    (SELECT COUNT(*) 
     FROM ECOLI_DATA 
     WHERE PARENT_ID = ED.ID) AS CHILD_COUNT
FROM ECOLI_DATA ED
ORDER BY ID ASC;
```
- 올바른 사고의 흐름
  - 이 문제를 풀 때는 "부모 입장에서 자식이 몇 명인지"를 세어야 합니다.
  - 왼쪽 테이블(부모): 모든 대장균 리스트 (E1)
  - 오른쪽 테이블(자식): 누군가를 부모로 모시는 대장균 리스트 (E2)
  - 연결 조건: 부모의 ID = 자식의 PARENT_ID
  - 기준: 모든 부모(E1)를 다 보여줘야 하므로 LEFT JOIN
 
|유형|문제|코드|
|:--:|:--:|:--:|
|select|대장균의 크기에 따라 분류하기 1<br>https://school.programmers.co.kr/learn/courses/30/lessons/299307| CASE WHEN |



```sql
-- CASE WHEN  기본 문법 구조
SELECT 
    CASE 
        WHEN 조건1 THEN 결과1
        WHEN 조건2 THEN 결과2
        ELSE 결과3 -- 모든 조건에 해당하지 않을 때 (생략 가능, ELSE 생략 시 NULL)
    END AS 별칭
FROM 테이블명;
```
```SQL
SELECT ID,
    CASE 
    WHEN SIZE_OF_COLONY	 <=100 THEN 'LOW'
    WHEN SIZE_OF_COLONY	 <=1000 THEN 'MEDIUM'
    ELSE 'HIGH' 
    END AS SIZE

FROM ECOLI_DATA
ORDER BY ID
```
