- [x] SQL: 조건에 맞는 사원 정보 조회

# SQL: 조건에 맞는 사원 정보 조회

## 소개
MySQL을 사용하여 조건에 따라 사원 정보를 조회하는 방법을 학습하며, 발생할 수 있는 주요 오류와 이를 해결하는 방안을 정리함.

---

## 주요 학습 내용

### 1. 서브쿼리와 `LIMIT` 오류
#### 문제 정의
- MySQL에서 다음 오류가 발생할 수 있음:
  ```
  This version of MySQL doesn't yet support 'LIMIT & IN/ALL/ANY/SOME subquery'
  ```
- **원인**:
  - MySQL은 `LIMIT` 절과 `IN` (또는 `ALL`, `ANY`, `SOME`) 서브쿼리를 함께 사용할 수 없음.

#### 해결 방법
1. **`WITH` 절 사용**:
   - 서브쿼리 결과를 임시 테이블로 저장하여 `LIMIT`을 우회.

#### 예제 코드
```sql
WITH temp_table AS (
    SELECT employee_id, employee_name
    FROM employees
    WHERE department = 'Sales'
    ORDER BY hire_date DESC
    LIMIT 10
)
SELECT *
FROM temp_table
WHERE employee_id IN (SELECT manager_id FROM managers);
```
- **설명**:
  - `WITH` 절로 임시 테이블을 생성한 뒤, 이를 활용하여 `LIMIT`과 `IN` 조건을 분리.



## 깨달은 것
1. . **조건 설정의 중요성**:
   - SQL 조건문(`HAVING`, `WHERE`, `BETWEEN`)을 적절히 사용하여 원하는 데이터를 효율적으로 조회 가능.

SQL 쿼리 작성과 최적화를 통해 데이터 조회 효율성과 문제 해결 능력을 키울 수 있었음.

