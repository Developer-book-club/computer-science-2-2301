# Chapter 4 DataBase(데이터베이스)

# Section 4.6 조인(Join)의 종류

## 📌 조인(Join)

> 💡 조인(Join)은 하나의 테이블이 아닌 두 개 이상의 테이블을 묶어 하나의 조회 결과를 만드는 것을 뜻합니다.
>

## 1️⃣ 내부 조인(Inner Join)

> 💡 내부 조인(Inner Join)은 왼쪽 테이블과 오른쪽 테이블의 두 행이 모두 일치하는 행이 있는 부분만 표기합니다.
>
내부 조인은 두 테이블 간의 `교집합`을 나타냅니다.

``` sql
SELECT * FROM Table_A as ta
INNER JOIN Table_B as tb
  ON ta.tableId = tb.tableId;
```

- `Table_A` 테이블과 `Table_B` 테이블을 내부 조인하여 데이터를 조회합니다.

## 2️⃣ 왼쪽 조인(Left Outer Join)

> 💡 왼쪽 조인(Left Outer Join)은 왼쪽 테이블의 모든 행이 결과 테이블에 표기됩니다.
>

왼쪽 조인은 테이블 B의 일치하는 부분의 레코드와 테이블 A를 기준으로 레코드 집합을 생성합니다.

``` sql
SELECT * FROM Table_A as ta
LEFT JOIN Table_B as tb
  ON ta.tableId = tb.tableId;
```

## 3️⃣ 오른쪽 조인(Right Outer Join)

> 💡 오른쪽 조인(Right Outer Join)은 오른쪽 테이블의 모든 행이 결과 테이블에 표기됩니다.
>

오른쪽 조인은 테이블 A에서 일치하는 부분의 레코드와 함께 테이블 B를 기준으로 레코드 집합을 생성합니다.

``` sql
SELECT * FROM Table_A as ta
RIGHT JOIN Table_B as tb
  ON ta.tableId = tb.tableId;
```

## 4️⃣ 합집합 조인(Full Outer Join)

> 💡 합집합 조인(Full Outer Join)은 두 개의 테이블을 기반으로 조인 조건에 만족하지 않는 모든 행까지 모두 표기합니다.
>
합집합 조인(완전 외부 조인)은 양쪽 테이블에 일치하는 레코드와 함께 테이블 A와 테이블 B의 모든 레코드 집합을 생성합니다.

``` sql
SELECT * FROM Table_A as ta
FULL OUTER JOIN Table_B as tb
  ON ta.tableId = tb.tableId;
```