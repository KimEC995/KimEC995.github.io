---
title: SQL실습- Churn Rate 구하기(비활동 비율 구하기)
author: Kimec995
date: 2023-10-06 00:13:00 +09:00
categories: [SQL, Apply]
tags: [SQL, MySQL, Basic]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/SQL_ALL/SQL_ALL_mySQL
  alt: image alternative text
---

SQL 문법을 몇 가지 살펴봤었다.

문제를 풀어보자.


## 문제: 전체 고객 중 비활성 고객 비율 구하기

Chrun은 비즈니스 용어로, 
기존 고객이 더 이상 회사의 제품이나 서비스를 이용하지 않고 떠나는 현상을 나타냄.

<span style="color:black; background-color:#fff5b1;">**즉, 고객 이탈 또는 고객 유실을 의미함**</span>

따라서 `최근구매일` 과 `특정일` 의 차이가 일정 기간을 넘어가면 `비활성고객` 으로 판단하는 것이다.

사용하는 데이터는 [MYSQL SampleDATA](https://www.mysqltutorial.org/mysql-sample-database.aspx)인데..

차량 구매 데이터라 기간을 기준으로 찾는게 의미가 있나 싶기도 함.

일단

전체 플로우를 생각해보면.

---

1. **모든 고객의 최근 주문일을 확인한다.** 
    - 이는 DB내의 데이터의 최신 날짜를 확인하기 위함이다.(옛날 데이터라...)

2. **각 고객의 최근 주문일을 확인한다.**
    - 최근 주문은 DATE 중 가장 큰 값일 것이다. `MAX()`

3. **각 고객의 최근 주문일과 설정일의 차이를 확인한다.**
    - 기간의 차이를 확인하는 함수는 `DATEDIFF()`

4. **설정일이 넘어가면 '비활성', 안넘으면 '활성' 으로 표시한다.**
    - `CASE WHEN()` 으로 분류하기

5. **전체 고객 수 중 '비활성' 고객의 비율을 확인한다.**
    - 전체를 `COUNT(*)` , '비활성' 은 `COUNT('비활성')`

6. **번외) '비활성' 고객이 마지막으로 구매 한 품목은?**
    - ERD를 확인해 품목의 Table을 찾고 JOIN을 사용하자.

---
## 0. ERD

ERD를 확인해보자.

![image.png](\assets\img\postimg\SQL_ALL\SQL_ALL_00.png)

필요한건

- 고객 번호
- 구매일자
- 번외) 품명

이다.

**orders** 테이블에서 '고객번호:customerNumber' 와 '구매일자:orderdate' 를 추출해 비활성 고객을 찾고 

번외는 그 이후 본다.

## 1. 모든 고객의 최근 주문일을 확인한다.

우선 기준일 탐색을 위해 모든 고객들의 최근 주문일과 최초 구매일을 확인하자.

``` sql
SELECT
	MAX(orderdate) AS mx_order -- 최근 구매일
    , MIN(orderdate) AS mn_order -- 최초 구매일
FROM orders
;
```

DATE 값 중 가장 큰 값이 최근일 것이고, 가장 작은 값이 최초 구매일 일 것이다.

![image.png](\assets\img\postimg\SQL_Q1\SQL_Q1_01.png)

최근은 2005년 05월 31일

최초는 2003년 01월 06일 이다.

현재 날짜를 기준일로 설정할 수 있겠으나, 그러면 각 주문 별 차이가 비슷해 지기 때문에 최근 구매일 +1일로 기준일을 설정하자.

기준일: 2005년 06월 01일

## 2. 각 고객의 최근 주문일을 확인한다.

이 문제에서 필요한건 전체 최근 주문일이 아니라, 각 고객의 최근 주문일이다.

그에 맞게 코드를 짜면

```sql
SELECT
	customerNumber
	, MAX(orderdate) AS mx_order
FROM orders
GROUP BY 1
LIMIT 5
;
```
![image.png](\assets\img\postimg\SQL_Q1\SQL_Q1_02.png)

이런 식으로 고객번호 - 주문일 형식을 만들었다.

## 3. 각 고객의 최근 주문일과 설정일의 차이를 확인한다.

`DATEDIFF()`를 사용해 기준일과 최근 주문일의 차이를 만들자.

```sql
SELECT
	customerNumber
	, MAX(orderdate) AS mx_order
    , DATEDIFF('2005-06-01',MAX(orderdate)) AS DIF
FROM orders
GROUP BY 1
LIMIT 5
;
```
![image.png](\assets\img\postimg\SQL_Q1\SQL_Q1_03.png)

고객번호 - 최근 주문일 - 설정일과의 기간차이

를 만들었다.

이제 이 기간차이를 가지고 고객을 분류하자.

## 4. 설정일이 넘어가면 '비활성', 안넘으면 '활성' 으로 표시한다.

`CASE WHEN()` 을 사용해 '비활성' 과 '활성' 으로 구분하고자 한다.

일반적으로 구분 기준은 3개월 이다.

하지만 현재 데이터는 '자동차 구매 고객' 데이터 이다.

보통은 자동차를 3개월마다 바꾸지는 않아 기준으로는 좀 애매해 보이는데 기간차이의 평균을 측정해 기준일을 세워보자.

아마 실무에선 기준일의 정의가 있을 듯 한데 개인 실습이니 자유롭게 기준일을 설정 해 보자.

### 기간차이 기준일 찾기

```sql
SELECT AVG(DIF)
FROM (
	SELECT
		customerNumber
		, MAX(orderdate) AS mx_order
		, DATEDIFF('2005-06-01',MAX(orderdate)) AS DIF
	FROM orders
	GROUP BY 1
) BASE
;
```

`DATEDIFF` 와 `AVG` 함수를 같이 사용할 수 없어 서브쿼리를 사용했다.

![image.png](\assets\img\postimg\SQL_Q1\SQL_Q1_04.png)

현재 데이터 상 자동차 재 구매 기간의 평균은 183.7일 이다.

여기선 약 반년에 한번씩 자동차를 바꾸는 모양이다.. 부럽다.

기준: 180일

### 기준일로 분류하기.

기준일을 만들었겠다, 고객을 분류하자!

```sql
SELECT 
	*
	,CASE WHEN DIF >=180
    THEN 'CHURN'
	ELSE 'NONCHURN'
	END AS CHECK_D
FROM (
	SELECT
		customerNumber
		, MAX(orderdate) AS mx_order
		, DATEDIFF('2005-06-01',MAX(orderdate)) AS DIF
	FROM orders
	GROUP BY 1
) BASE
LIMIT 5
;
```
위와 마찬가지로 `CASE WHEN()` 을 사용하기 위해 서브쿼리로 만들었다.

각 고객의 기간차이를 파악해 180일이 넘어가면 'CHURN' / 안넘어가면 'NONCHURN' 으로 분류했다.

![image.png](\assets\img\postimg\SQL_Q1\SQL_Q1_05.png)

## 5.전체 고객 수 중 'CHURN' 고객의 비율을 확인한다.

아마 평균을 기준으로 분류해서 드라마틱한 결과는 아닐 것 같지만... 비율을 한번 확인 해보자.

`COUNT()` 와 `CASE WHEN()` 을 활용하자!

```sql
SELECT 
	COUNT(*) AS ALL_Count
    , ( (SUM(CASE WHEN CHECK_D = 'CHURN' THEN 1 ELSE 0 END)) / (COUNT(*)) ) AS Rate
FROM (
	SELECT 
		*
		,CASE WHEN DIF >=180
		THEN 'CHURN'
		ELSE 'NONCHURN'
		END AS CHECK_D
	FROM (
		SELECT
			customerNumber
			, MAX(orderdate) AS mx_order
			, DATEDIFF('2005-06-01',MAX(orderdate)) AS DIF
		FROM orders
		GROUP BY 1
	) BASE
) BASE
LIMIT 5
;
```

역시 다시 한번 `CASE WHEN()` 을 사용하기 위해 서브쿼리를 이용했다.

`COUNT`( `CASE WHEN`(비활성인경우) ) 을 하지 않은 이유는 두 함수는 같이 사용할 수 없기 때문이다.

그래서 비활성인 경우는 1 / 활성은 0 으로 바꾼 후 그 열을 모두 `SUM()` 하면 `COUNT()` 와 같은 결과가 나올 것이라 판단, 작성했다.

![image.png](\assets\img\postimg\SQL_Q1\SQL_Q1_06.png)

## 결론.

98명의 고객 중 반년 내에 재구매 하지 않은 비율은 약 `54%`라는 결론이 나온다.

---

## 번외) '비활성' 고객이 마지막으로 구매 한 품목은?

결론을 보니 궁금해진다.

비활성 고객은 마지막에 어떤 자동차를 샀길래 그 이후 구매를 안한걸까?

교재에선 자동차의 카테고리만 확인하는데, 난 궁금하니까 품목을 보려 한다.

### 번-0. ERD 확인하기
![image.png](\assets\img\postimg\SQL_ALL\SQL_ALL_00.png)

현재 작업은 **orders** 을 활용해 'customerNumber'가 있는 테이블이고 필요한 데이터는 **products** 테이블의 **productName** 열 이다.

'customerNumber' 에서 바로 **products** 을 JOIN 할 수가 없으니 **orders**를 사용해 중간에 **orderdetail** 을 거쳐 갈 것이다.

### 번-1. 비구매 고객 테이블 만들기

이전에 만든 데이터는 이후에 필요없는 자료들도 있고 순서도 보기가 어렵다. 재정비하고 코드의 간소를 위해 테이블을 만들자.

```sql
CREATE TABLE C_list AS
SELECT 
	CASE WHEN DIF >=180
	THEN 'CHURN'
	ELSE 'NONCHURN'
	END AS CHECK_D
    , customerNumber
FROM (
	SELECT
		customerNumber
		, MAX(orderdate) AS mx_order
		, DATEDIFF('2005-06-01',MAX(orderdate)) AS DIF
	FROM orders
	GROUP BY 1
) BASE
;

-- 확인하기
SELECT * FROM C_list;
```

CHECK_D 와 customerNumber 열만 남기고 테이블을 생성했다.

### 번-2. 테이블 JOIN

이제 **orders** + **orderdetails** + **products** 를 `JOIN()` 해야 하는데, 필요한 열은 **orders**의 'customerNumber' 와 **products**의 'productName' 이고 이 둘을 연결 할 테이블은 **orderdetails** 이다.

따라서 순서를 

- **orderdetails** : A
    - 'productCode'
    - 'orderNumber'

- **orders** : B
    - 'customerNumber'
    - 'orderNumber'

- **products** : C
    - 'productCode'
    - 'productName'

- **C_list(만든거)** : D
    - 'customerNumber'
    - 'CHECK_D'

로 한다.

```sql
SELECT
	D.customerNumber
    , D.CHECK_D
    , C.productName
FROM orderdetails A
LEFT JOIN orders B
ON A.orderNumber = B.orderNumber
LEFT JOIN products C
ON A.productCode = C.productCode
LEFT JOIN C_list D
ON B.customerNumber = D.customerNumber
LIMIT 5
;
```
![image.png](\assets\img\postimg\SQL_Q1\SQL_Q1_07.png)

### 번-3. CHURN(비활성) 고객의 최다 구매 품목 확인

1. 비활성만 `WHERE CHECK_D = 'CHURN'`
2. `COUNT(CHECK_D)` 개수확인
3. 랭킹 `RANK()OVER()`
4. TOP 10개 출력

```sql
SELECT
	PN
    , CNT
    , RNK
FROM (
	SELECT
		*
		, RANK()OVER(ORDER BY CNT DESC) AS RNK
	FROM (
		SELECT
			D.CHECK_D AS CD
			, C.productName AS PN
			, COUNT(CHECK_D) AS CNT
		FROM orderdetails A
		LEFT JOIN orders B
		ON A.orderNumber = B.orderNumber
		LEFT JOIN products C
		ON A.productCode = C.productCode
		LEFT JOIN C_list D
		ON B.customerNumber = D.customerNumber
		WHERE CHECK_D = 'CHURN'
		GROUP BY 2
	) BASE
)BASE
WHERE RNK <= 10
;
```

![image.png](\assets\img\postimg\SQL_Q1\SQL_Q1_08.png)

1992 Ferrari 360 Spider red 라는 자동차가 가장 많이 팔렸다.

![image.png](\assets\img\postimg\SQL_Q1\SQL_Q1_09.png)

[사진출처](https://carsandbids.com/auctions/rMWL18w4/2000-ferrari-360-spider)

멋진 자동차다!

## 참고자료 및 데이터

[서적: SQL로 맛보는 전처리 분석](https://product.kyobobook.co.kr/detail/S000001934242)

[MYSQL SampleDATA](https://www.mysqltutorial.org/mysql-sample-database.aspx)