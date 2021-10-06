---
title: "MySQL 데이터베이스의 Row 개수와 테이블 크기 구하기"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-03-17T01:00:30+09:00
categories:
  - MySQL
tags:
  - MySQL
  - Database
permalink: /mysql/size/
---
---
{% include ad-contents.html %}


MySQL의 기본 데이터베이스인, [INFORMATION_SCHEMA](https://dev.mysql.com/doc/refman/8.0/en/information-schema.html){:target="_blank"} 의 [TABLES](https://dev.mysql.com/doc/refman/8.0/en/information-schema-tables-table.html){:target="_blank"} 테이블에는 각각의 테이블에 대한 정보가 들어있습니다. SELECT 문으로 필요한 정보만 가져와보도록 하겠습니다.
<!--more-->

## 쿼리문
아래의 SQL 쿼리에서 fancy_data 를 원하는 데이터베이스 이름으로 바꿔주기만 하면 됩니다.
```sql
SELECT TABLE_NAME "Tables",
    ROUND((TABLE_ROWS / 1000), 3) "Rows (K)",
    AVG_ROW_LENGTH "Row Size (Byte)",
    ROUND((DATA_LENGTH / 1024 / 1024), 3) "Data Size (MB)",
    ROUND((INDEX_LENGTH / 1024 / 1024), 3) "Index Size (MB)",
    ROUND(((DATA_LENGTH + INDEX_LENGTH) / 1024 / 1024), 3) "Total Size (MB)"
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_SCHEMA = "fancy_data"  /* 데이터베이스 이름 */
ORDER BY (DATA_LENGTH + INDEX_LENGTH) DESC;
```

## 결과
![Result](/assets/post-images/mysql-size/result.png)

## 쿼리 설명

### SELECT ([AS는 생략가능](https://ko.wikipedia.org/wiki/Select_(SQL)){:target="_blank"})
```sql
SELECT 가져올_데이터 AS "컬럼명", /*** AS는 ***/
    가져올_데이터2 AS "컬럼명2",  /* 생략가능 */
    ...
```
### [ROUND](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_round){:target="_blank"}
```sql
ROUND((123.45678), 3) /* 소수점 3자리 밑에서 반올림 */
  /* -> 123.457 */    /******** 음수도 가능! ********/
```

### FROM, WHERE, ORDER BY
```sql
FROM 데이터베이스이름.테이블이름 /* 데이터를 가져올 테이블 */
WHERE 데이터 = 값     /* 조건문 (일치하는 데이터 가져옴) */
ORDER BY 데이터 DESC  /* 데이터 순서로 정렬 (DESC는 내림차순) */
```

{% include ad-contents.html %}

## Reference
* [MySQL 테이블 및 데이타베이스 크기 알아내기](https://www.lesstif.com/dbms/mysql-17105786.html){:target="_blank"}

* [Chapter 26 INFORMATION_SCHEMA Tables](https://dev.mysql.com/doc/refman/8.0/en/information-schema.html){:target="_blank"}

* [26.38 The INFORMATION_SCHEMA TABLES Table](https://dev.mysql.com/doc/refman/8.0/en/information-schema-tables-table.html){:target="_blank"}

* [Select (SQL)](https://ko.wikipedia.org/wiki/Select_(SQL)){:target="_blank"}

* [SQL AS Keyword](https://www.w3schools.com/sql/sql_ref_as.asp){:target="_blank"}

* [12.6.2 Mathematical Functions](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html){:target="_blank"}