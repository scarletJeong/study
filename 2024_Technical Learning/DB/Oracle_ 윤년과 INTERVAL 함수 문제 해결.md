- [x] Oracle INTERVAL 함수 윤년 문제와 해결 방법

# Oracle: 윤년과 INTERVAL 함수 문제 해결

## 소개
Oracle Database에서 INTERVAL 함수 사용 시 윤년과 관련된 에러(`ORA-01839`)가 발생할 수 있음. 이 문서에서는 이러한 문제의 원인을 분석하고, `ADD_MONTHS()`를 활용한 해결 방법을 정리함.

## 주요 학습 내용

### 1. 문제 정의
- **문제 상황**:
  - INTERVAL YEAR/MONTH를 사용하는 경우 윤달(2월 29일) 계산 시 에러 발생.
  - 예: `TO_DATE('2024-02-29') - INTERVAL '1' YEAR` 실행 시 `ORA-01839` 에러 발생.

- **발생 에러**:
  ```sql
  SELECT TO_DATE('2024-02-29', 'YYYY-MM-DD') - INTERVAL '1' YEAR
  FROM DUAL;
  -- ORA-01839: 지정된 월에 대한 날짜가 부적합합니다
  ```

---

### 2. 해결 방법: ADD_MONTHS() 사용
- **ADD_MONTHS() 함수**:
  - INTERVAL YEAR/MONTH 대신 ADD_MONTHS() 함수로 연산 수행.
  - 날짜를 안전하게 계산하며, 윤년을 포함한 모든 날짜를 올바르게 처리.

- **수정 예시**:
  ```sql
  SELECT ADD_MONTHS(TO_DATE('2024-02-29', 'YYYY-MM-DD'), -12)
  FROM DUAL;
  -- 결과: 2023-02-28
  ```

#### 주요 쿼리 비교
1. **문제가 발생하는 쿼리**:
   ```sql
   SELECT TO_DATE('2024-02-29', 'YYYY-MM-DD') - INTERVAL '1' YEAR
   FROM DUAL;
   -- ORA-01839 발생
   ```

2. **해결된 쿼리**:
   ```sql
   SELECT ADD_MONTHS(TO_DATE('2024-02-29', 'YYYY-MM-DD'), -12)
   FROM DUAL;
   -- 결과: 2023-02-28
   ```

---

### 3. 응용 사례
#### 1. 간호사 교대제 동의서 취득 여부 확인 쿼리
- **수정 전**:
  ```sql
  AND pecm.rgstdd >= TO_CHAR(SYSDATE - (INTERVAL '5' YEAR), 'YYYYMMDD')
  ```

- **수정 후**:
  ```sql
  AND pecm.rgstdd >= TO_CHAR(ADD_MONTHS(SYSDATE, -12*5), 'YYYYMMDD')
  ```

#### 2. 과거 데이터 조회
- **윤년 문제가 발생한 쿼리**:
  ```sql
  SELECT TO_DATE('2023-03-31', 'YYYY-MM-DD') - INTERVAL '1' MONTH
  FROM DUAL;
  -- ORA-01839 발생
  ```

- **ADD_MONTHS로 수정**:
  ```sql
  SELECT ADD_MONTHS(TO_DATE('2023-03-31', 'YYYY-MM-DD'), -1)
  FROM DUAL;
  -- 결과: 2023-02-28
  ```

---

## 깨달은 것
1. **INTERVAL의 한계**:
   - 윤년 또는 날짜의 경계 조건을 처리하지 못하는 경우가 있음.
2. **ADD_MONTHS()의 안정성**:
   - 날짜 연산에서 유연하게 윤년과 월말 데이터를 처리 가능.
3. **SQL 설계 시 주의점**:
   - 윤년과 같이 날짜 경계값이 포함된 연산에서는 안전한 함수 사용이 필수적임.

ADD_MONTHS()를 사용하여 안정적인 날짜 연산을 구현함으로써 데이터 무결성과 애플리케이션의 안정성을 확보할 수 있음.

[2024-03-04]
