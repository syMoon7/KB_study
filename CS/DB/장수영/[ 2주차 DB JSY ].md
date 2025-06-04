# [ 2주차 | DB | JSY ]

# 📝데이터베이스

## 6.3 SQL

앞서 살펴봤던 RDBMS에서 사용하는 SQL 명령은 크게 **데이터 정의 언어 (DDL)**, 데이터 조작 언어(DML), 데이터 제어 언어(DCL), 트랜잭션 제어 언어(TCL)로 나눠진다. 각 언어를 순서대로 정리해보겠다.

- 데이터 정의 언어(DDL)

데이터 정의를 위한 SQL인 DDL은 말 그대로 데이터를 작성하기전 데이터베이스의 생성, 갱신, 삭제를 의미

| 종류                                         | 설명                                      |
| -------------------------------------------- | ----------------------------------------- |
| CREATE                                       | 데이터 베이스 혹은 데이터베이스 객체 생성 |
| ALTER                                        | 데이터베이스 객체 갱신                    |
| (예: 테이블에 필드 및 제약 조건을 추가/삭제) |
| DROP                                         | 데이터베이스 객체 삭제                    |
| (예: 테이블이나 데이터베이스를 삭제)         |
| TRUNCATE                                     | 테이블 구조를 유지한 채 모든 레코드 삭제  |

---

## ✅ DDL(Data Definition Language)

- CREATE

CREATE 명령은 데이터베이스, 테이블, 뷰, 사용자까지 데이터베이스에서 관리될 수 있는 대상을 생성하는 역할을 담당.

- CREATE 명령어를 사용해 데이터베이스 생성 예제

```sql
CREATE DATABASE 데이터베이스_이름;
```

- ALTER

ALTER 명령은 CREATE 문을 통해 생성된 테이블에 새로운 필드를 추가하거나 기존의 필드를 수정/삭제할 수 있고, 제약 조건 또한 추가하거나 수정/삭제 가능.

- ALTER 명령어를 사용해 데이터베이스 수정/삭제 예제

```sql
-- 새로운 필드 추가
ALTER TABEL 테이블 이름 ADD COLUMN 필드_이름 필드_타입 [제약조건]
ALTER TABLE users ADD COLUMN name VARCHAR(50) NOT NULL;

-- 기존 필드 수정
ALTER TABEL 테이블 이름 CHANGE COLUMN 기존_필드_이름 새_필드_이름 필드_타입 [제약조건]
ALTER TABLE users CHANGE COLUMN pre_field new_field VARCHAR(100) NOT NULL;

-- 기존 필드 삭제
ALTER TABLE 테이블 이름 DROP COLUMN 필드 이름;
ALTER TABLE users DROP COLUMN name;

-- 외래키 제약 조건 추가
ALTER TABLE orders
ADD CONSTRAINT fk_customer_id
FOREIGN KEY (customer_id) REFERENCES customers(id);

-- UNIQUE 제약 조건 추가
ALTER TABLE users
ADD CONSTRAINT uniq_email
UNIQUE (email);

-- NOT NULL 제약 조건 추가
ALTER TABLE employees
MODIFY COLUMN phone_number VARCHAR(20) NOT NULL;

-- 기본 키 설정(PK가 설정되어 있는 필드가 없는 경우)
ALTER TABLE products
ADD CONSTRAINT pk_product_id
PRIMARY KEY (product_id);

```

- DROP

DROP 명령은 테이블이나 데이터베이스를 삭제할 수 있는 명령어이다.

```sql
-- users 데이터베이스를 삭제하는 예제
DROP DATABASE users;

-- users 테이블을 삭제하는 예제
DROP TABLE users;
```

- TRUNCATE

TRUNCATE명령은 테이블의 구조를 유지한 채로 테이블의 모든 레코드를 삭제한다.

즉 테이블의 레코드(데이터)는 모두 삭제하되, 테이블 자체는 유지하는 것을 말한다.

```sql
-- users 테이블 TRUNCATE 사용 예제
TRUNCATE TABLE users;
```

---

## ✅ DML (Data Manipulation Language)

DML은 **데이터 조작 언어**로, 테이블에 데이터를 **삽입, 조회, 수정, 삭제**하는 데 사용됩니다.

- **INSERT**
  테이블에 새 데이터를 추가하는 명령어
  ```sql

  INSERT INTO users (id, name, email) VALUES (1, '홍길동', 'hong@example.com');
  ```
- **SELECT**
  테이블에서 데이터를 조회하는 명령어
  ```sql

  SELECT * FROM users;
  ```
- **UPDATE**
  기존 데이터를 수정하는 명령어
  ```sql

  UPDATE users SET name = '이순신' WHERE id = 1;
  ```
- **DELETE**
  기존 데이터를 삭제하는 명령어
  ```sql

  DELETE FROM users WHERE id = 1;
  ```
  ## ✅ DML 내 조건/정렬 절
  ***
  ### 🔹 `GROUP BY`
  `GROUP BY`는 특정 컬럼을 기준으로 **데이터를 그룹화**할 때 사용, 일반적으로 **집계 함수(SUM, COUNT, AVG 등)**와 함께 사용
  ```sql

  -- 부서별 직원 수 조회
  SELECT department, COUNT(*) AS employee_count
  FROM employees
  GROUP BY department;

  ```
  ***
  ### 🔹 `HAVING`
  `HAVING`은 `GROUP BY` 절로 **그룹화된 결과에 조건을 걸 때** 사용
  ※ `WHERE`은 개별 행에 조건, `HAVING`은 그룹에 조건.
  ```sql

  -- 직원 수가 5명 이상인 부서만 조회
  SELECT department, COUNT(*) AS employee_count
  FROM employees
  GROUP BY department
  HAVING COUNT(*) >= 5;

  ```
  ***
  ### 🔹 `ORDER BY`
  `ORDER BY`는 **조회 결과를 정렬**할 때 사용, 기본값은 오름차순(`ASC`)이며, 내림차순은 `DESC`를 사용
  ```sql

  -- 직원 정보를 이름순으로 정렬
  SELECT * FROM employees
  ORDER BY name ASC;

  -- 급여 기준으로 내림차순 정렬
  SELECT * FROM employees
  ORDER BY salary DESC;

  ```
  ***
  ### 🔹 함께 사용하는 예제
  ```sql

  -- 부서별 평균 급여가 5000 이상인 부서만, 평균 급여 기준으로 내림차순 정렬
  SELECT department, AVG(salary) AS avg_salary
  FROM employees
  GROUP BY department
  HAVING AVG(salary) >= 5000
  ORDER BY avg_salary DESC;

  ```
  ### ❓WHERE VS HAVING 차이점
  ## ✅ `WHERE` vs `HAVING` 차이
  | 구분               | `WHERE`                                             | `HAVING`                        |
  | ------------------ | --------------------------------------------------- | ------------------------------- |
  | **적용 대상**      | **행(로우)**에 조건 적용                            | **그룹(집계 결과)**에 조건 적용 |
  | **적용 시점**      | **`GROUP BY` 전에 실행됨**                          | **`GROUP BY` 후에 실행됨**      |
  | **집계 함수 사용** | ❌ 직접 사용 불가 (`COUNT()`, `SUM()` 등 사용 불가) | ✅ 집계 함수 조건 가능          |
  | **주 용도**        | 데이터 필터링                                       | 그룹핑된 결과 필터링            |
  ## 📌 예제 비교
  ### 1. `WHERE` 사용 – **개별 행 필터링**
  ```sql

  -- 급여가 3000 이상인 직원만 조회
  SELECT * FROM employees
  WHERE salary >= 3000;
  ```
  > ▶ 개별 직원(row) 중에서 salary >= 3000 조건을 만족하는 경우만 선택
  ### 2. `HAVING` 사용 – **그룹 필터링**
  ```sql

  -- 부서별 평균 급여가 5000 이상인 부서만 조회
  SELECT department, AVG(salary) AS avg_salary
  FROM employees
  GROUP BY department
  HAVING AVG(salary) >= 5000;
  ```
  > ▶ 부서별로 그룹화한 후, 그 **집계 결과(평균 급여)**에 조건을 적용
  ### 3. 함께 사용하는 예제
  ```sql

  -- 급여가 3000 이상인 직원 중에서, 부서별 평균 급여가 5000 이상인 부서만
  SELECT department, AVG(salary) AS avg_salary
  FROM employees
  WHERE salary >= 3000
  GROUP BY department
  HAVING AVG(salary) >= 5000;

  ```
  ### ⚠️ `HAVING` 절 사용 시 주의점
  1️⃣ `HAVING`은 **집계 함수와 함께** 사용하는 것이 일반적
  ✅ 올바른 예:
  ```sql

  SELECT department, COUNT(*) AS emp_count
  FROM employees
  GROUP BY department
  HAVING COUNT(*) > 5;
  ```
  2️⃣`WHERE` → `GROUP BY` → `HAVING` → `ORDER BY` 순서로 실행됨
  SQL 실행 순서를 이해하지 못하고 `HAVING`을 먼저 쓰거나 집계 전에 필터를 하려고 하면 **잘못된 결과**가 나올 수 있다.
  ```sql

  SELECT department, AVG(salary)
  FROM employees
  WHERE salary > 3000          -- 먼저 row를 필터링
  GROUP BY department
  HAVING AVG(salary) >= 5000;  -- 그 후 그룹 조건 적용
  ```

---

## ✅ DCL (Data Control Language)

DCL은 **데이터 제어 언어**로, 사용자에게 **권한을 부여하거나 회수**하는 데 사용됩니다.

- **GRANT**
  사용자에게 권한을 부여하는 명령어
  ```sql

  GRANT SELECT, INSERT ON users TO 'john'@'localhost';
  ```
- **REVOKE**
  사용자로부터 권한을 회수하는 명령어
  ```sql

  REVOKE INSERT ON users FROM 'john'@'localhost';
  ```

---

## ✅ TCL (Transaction Control Language)

TCL은 **트랜잭션 제어 언어**로, DML 작업을 **묶어서 처리**하거나 **롤백**하는 데 사용됩니다.

- **COMMIT**
  트랜잭션에서 실행된 모든 변경 사항을 **영구 저장**하는 명령어
  ```sql

  COMMIT;
  ```
- **ROLLBACK**
  트랜잭션 내의 변경 사항을 **되돌리는** 명령어
  ```sql

  ROLLBACK;
  ```
- **SAVEPOINT**
  트랜잭션 내 특정 시점에 저장점을 설정하는 명령어
  ```sql

  SAVEPOINT sp1;

  ```
- **ROLLBACK TO SAVEPOINT**
  특정 저장점까지 트랜잭션을 되돌리는 명령어
  ```sql

  ROLLBACK TO sp1;
  ```

---

## 6.4 효율적 쿼리

## ✅ 서브쿼리 (Subquery)

### 📌 개념

서브쿼리는 **다른 쿼리 안에 포함된 쿼리**로, 보통 괄호 `()` 안에 작성된다.

주로 **SELECT, WHERE, FROM 절**에서 사용되며, **중첩 쿼리**라고도 부른다.

---

### 🧩 서브쿼리 유형

### 1️⃣ **스칼라 서브쿼리** (단일 값 반환)

```sql

-- 직원 중 최고 급여를 받는 직원 조회
SELECT name, salary
FROM employees
WHERE salary = (SELECT MAX(salary) FROM employees);
```

### 2️⃣ **인라인 뷰 (FROM 절 서브쿼리)**

```sql

-- 각 부서의 평균 급여보다 높은 급여를 받는 직원 조회
SELECT e.name, e.salary, e.department
FROM employees e
JOIN (
  SELECT department, AVG(salary) AS avg_salary
  FROM employees
  GROUP BY department
) avg_dept
ON e.department = avg_dept.department
WHERE e.salary > avg_dept.avg_salary;

```

### 3️⃣ **다중 행 서브쿼리** (`IN`, `ANY`, `ALL` 등 사용)

```sql

-- '서울'에 있는 부서의 직원만 조회
SELECT name
FROM employees
WHERE department_id IN (
  SELECT id FROM departments WHERE location = '서울'
);

```

---

## ✅ 조인 (JOIN)

### 📌 개념

조인은 **두 개 이상의 테이블을 연결하여 관련 데이터를 함께 조회**하는 SQL 기능이다.

일반적으로 **외래 키**로 테이블 간 관계를 맺고 사용한다.

---

### 🧩 조인 유형

### 1️⃣ **INNER JOIN**

두 테이블의 **일치하는 값만** 반환

```sql

-- 직원과 부서 정보를 함께 조회
SELECT e.name, d.name AS department_name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.id;

```

### 2️⃣ **LEFT JOIN**

왼쪽 테이블은 **모두**, 오른쪽은 **일치하는 값만**

```sql

-- 모든 직원과 그들의 부서 (없는 경우 NULL)
SELECT e.name, d.name AS department_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.id;

```

### 3️⃣ **RIGHT JOIN**

오른쪽 테이블은 **모두**, 왼쪽은 **일치하는 값만**

```sql

-- 모든 부서와 그 부서에 속한 직원 (직원이 없으면 NULL)
SELECT e.name, d.name AS department_name
FROM employees e
RIGHT JOIN departments d
ON e.department_id = d.id;

```

### 4️⃣ **FULL OUTER JOIN** (MySQL은 직접 지원 X → `UNION`으로 구현)

```sql

-- 모든 직원과 부서 (일치하지 않아도 모두 포함)
SELECT e.name, d.name AS department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id
UNION
SELECT e.name, d.name AS department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;

```

---

## ✅ 인덱스 (Index)

### 📌 개념

인덱스는 데이터베이스에서 **검색 속도를 향상**시키기 위해 사용하는 **특별한 자료구조**이다**.**

> SELECT, WHERE, JOIN, ORDER BY 등에 자주 사용되는 컬럼에 인덱스를 걸면 성능 향상 가능.

---

## 🧩 인덱스의 종류

### 1️⃣ 기본 인덱스 (Single Column Index)

- 하나의 컬럼에 생성된 인덱스

```sql

CREATE INDEX idx_name ON employees(name);
```

---

### 2️⃣ 복합 인덱스 (Composite / Multi-Column Index)

- **여러 컬럼**을 묶어서 만든 인덱스
- **선두 컬럼 기준**으로 동작함

```sql

CREATE INDEX idx_dept_salary ON employees(department_id, salary);
```

> ✅ WHERE department_id = 10 → 사용 가능
>
> ❌ `WHERE salary = 5000` → 단독 사용 시 인덱스 **미사용**

---

### 3️⃣ 유니크 인덱스 (UNIQUE)

- **중복을 허용하지 않음** (제약 조건 포함)

```sql

CREATE UNIQUE INDEX idx_email ON users(email);
```

> = UNIQUE 제약 조건과 동일한 효과

---

### 4️⃣ 프라이머리 키 인덱스 (PRIMARY KEY)

- **기본 키** 지정 시 자동으로 생성됨

```sql

ALTER TABLE users ADD PRIMARY KEY (id);
```

---

### 5️⃣ 풀텍스트 인덱스 (FULLTEXT) – (MySQL)

- 긴 문자열 검색, 자연어 검색 등에서 사용

```sql

CREATE FULLTEXT INDEX idx_content ON posts(content);
```

> ⚠️ LIKE '%검색어%'에는 잘 안 맞음. MATCH...AGAINST 구문과 함께 사용.

---

## ✅ 인덱스의 장점과 단점

| 구분    | 설명                                                                                                |
| ------- | --------------------------------------------------------------------------------------------------- |
| ✅ 장점 | - 검색 속도 향상 (특히 대용량 테이블)- 정렬 성능 개선- WHERE, JOIN, ORDER BY 성능 최적화            |
| ⚠️ 단점 | - **쓰기 작업(INSERT, UPDATE, DELETE)** 성능 저하- 디스크 공간 추가 사용- 너무 많으면 오히려 역효과 |

---

## ⚠️ 인덱스 사용 시 주의사항

- **읽기 위주의 테이블**에 적합 (검색 많고 수정 적은 경우)
- **자주 사용하는 컬럼**에만 인덱스를 생성할 것
- **조건문에 자주 등장하는 컬럼**에 인덱스 적용
- \*정렬 기준 컬럼(ORDER BY)**이나 **JOIN 조건 컬럼\*\*도 인덱스 후보

---

## ✅ 인덱스 조회 & 삭제

```sql

-- 인덱스 목록 조회 (MySQL 기준)
SHOW INDEX FROM employees;

-- 인덱스 삭제
DROP INDEX idx_name ON employees;

```

---

## ✅ 인덱스와 실행 계획

```sql

-- 실행 계획으로 인덱스 사용 여부 확인 (MySQL)
EXPLAIN SELECT * FROM employees WHERE name = '홍길동';
```

> 📌 type: index / ref / range 등이 보이면 인덱스가 사용된 것
