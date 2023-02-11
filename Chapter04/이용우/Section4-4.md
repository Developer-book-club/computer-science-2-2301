# Chapter 4 DataBase(데이터베이스)

# Section 4.4 데이터베이스의 종류

## 📌 관계형 데이터베이스(RDBMS)

> 💡 관계형 데이터베이스(RDBMS)은 행(Row)과 열(Column)로 이루어진 테이블(Table)로 데이터를 관리하는 데이터베이스입니다.
>
관계형 데이터베이스는 대표적으로 MySQL, MariaDB, PostgreSQL, Oracle 등 다양한 DBMS가 존재합니다.

### MySQL

MySQL은 2022년 기준으로 가장 많이 사용하고 있는 데이터베이스입니다.  
Oracle과 동일한 기능을 가지고 있지만, 무료로 사용할 수 있다는 점에서 많은 사람들이 사용하고 있습니다.

## 📌 NoSQL 데이터베이스(Not Only SQL)

> 💡 NoSQL 데이터베이스(Not Only SQL)은 관계형 데이터베이스와는 다르게 테이블이 아닌 컬렉션(Collection)으로 데이터를 관리합니다.
>
NoSQL 데이터베이스는 대표적으로 MongoDB, Redis 등 다양한 DBMS가 존재합니다.

### MongoDB

MongoDB는 JSON 형식으로 데이터에 접근할 수 있고, BSON(Binary JSON) 형식으로 데이터를 저장합니다.  
대표적으로 Key-Value(키-값) 형식으로 데이터 모델을 관리하는 Redis와 같은 NoSQL 데이터베이스입니다.

### Redis

Redis는 인메모리 데이터베이스이고, Key-Value 데이터 모델 기반의 데이터베이스입니다.  
단순히 데이터를 저장하고, 출력하는것 뿐만 아니라, pub/sub(Publish/Subscibe) 방식으로 데이터를 전달할 수 있습니다.
