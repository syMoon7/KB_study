# 📝 데이터베이스 정리

## 📚 목차

### 6.1 데이터베이스의 큰 그림

- 📌 데이터베이스(Database)란?
- 📌 DBMS(Database Management System)
  - 🔸 대표적인 DBMS
- 📌 파일 시스템과 DBMS의 차이
- 📌 트랜잭션(Transaction)
  - 🔸 ACID: 트랜잭션의 4가지 성질

### 6.2 RDBMS의 기본

- 📌 RDBMS (Relational Database Management System)
- 📌 테이블(Table)
- 📌 기본 키(Primary Key)
- 📌 외래 키(Foreign Key)
- 📌 관계(Relationship)
- 📌 무결성 제약 조건 (Integrity Constraints)
- 📌 SQL 기초
  - 🔸테이블 생성
  - 🔸기본 조회
  - 🔸삽입
  - 🔸외래 키 제약

### 6.3 SQL

- 📌 SQL 언어 분류 개요
- 📌 DDL (Data Definition Language)
  - 🔸 CREATE
  - 🔸 ALTER
  - 🔸 DROP
  - 🔸 TRUNCATE
- 📌 DML (Data Manipulation Language)
  - 🔸 INSERT
  - 🔸 SELECT
  - 🔸 UPDATE
  - 🔸 DELETE
  - 🔸 GROUP BY
  - 🔸 HAVING
  - 🔸 ORDER BY
  - 🔸 WHERE vs HAVING
- 📌 DCL (Data Control Language)
  - 🔸 GRANT
  - 🔸 REVOKE
- 📌 TCL (Transaction Control Language)
  - 🔸 COMMIT
  - 🔸 ROLLBACK
  - 🔸 SAVEPOINT
  - 🔸 ROLLBACK TO SAVEPOINT

### 6.4 효율적 쿼리

- 📌 서브쿼리 (Subquery)

  - 🔸스칼라 서브쿼리
  - 🔸인라인 뷰(FROM 절 서브쿼리)
  - 🔸다중 행 서브쿼리

- 📌 조인(JOIN)
  - 🔸개념
- 📌 조인 유형

  - 🔸 INNER JOIN
  - 🔸 LEFT OUTER JOIN
  - 🔸 RIGHT OUTER JOIN
  - 🔸 FULL OUTER JOIN

- 📌 인덱스(INDEX)

  - 🔸기본 인덱스(Single Column Index)
  - 🔸복합 인덱스(Composite / Multi-Column Index)
  - 🔸유니크 인덱스(Unique Index)
  - 🔸프라이머리 인덱스(Primary Index)
  - 🔸풀텍스트 인덱스(FullText Index)

- 📌 인덱스의 장점과 단점
- 📌 인덱스 사용 시 주의사항
- 📌 인덱스 조회 & 삭제
- 📌 인덱스의 장점과 단점
- 📌 인덱스와 실행 계획

### 🔍 마무리 요약

- 핵심 키워드 요약 정리
