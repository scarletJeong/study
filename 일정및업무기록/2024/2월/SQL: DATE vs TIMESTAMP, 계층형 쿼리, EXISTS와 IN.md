- [x] SQL DATE/TIMESTAMP 처리 및 계층형 쿼리, EXISTS vs IN

# SQL DATE/TIMESTAMP 처리 및 계층형 쿼리, EXISTS vs IN 비교

## 소개
DATETIME 타입에서 시간(HOUR, MINUTE, SECOND) 값을 추출하는 방법과 SQL 계층형 쿼리 및 EXISTS와 IN 함수의 차이점을 정리함. 이를 통해 데이터베이스 작업 시 효율적이고 정확한 쿼리 작성 방법을 학습함.

## 주요 학습 내용

### 1. DATETIME과 EXTRACT 함수

#### 문제 상황
- Oracle DB에서 `DATE`와 `TIMESTAMP`의 차이를 이해해야 함.
  - **DATE**: 연도, 월, 일 정보만 포함.
  - **TIMESTAMP**: 시간(HOUR, MINUTE, SECOND) 정보도 포함.
- EXTRACT 함수는 시간 관련 데이터를 처리할 때, 데이터 타입이 TIMESTAMP여야 함.

#### 해결 방법
1. **DATE와 TIMESTAMP 구분**
   - 연도, 월, 일 값 추출: DATE 타입 사용.
   - 시간(HOUR, MINUTE, SECOND) 값 추출: TIMESTAMP로 변환 필요.

2. **EXTRACT 함수 사용법**
   ```sql
   SELECT EXTRACT(HOUR FROM CAST(DATETIME_COLUMN AS TIMESTAMP)) AS HOUR,
          EXTRACT(MINUTE FROM CAST(DATETIME_COLUMN AS TIMESTAMP)) AS MINUTE,
          EXTRACT(SECOND FROM CAST(DATETIME_COLUMN AS TIMESTAMP)) AS SECOND
     FROM SAMPLE_TABLE;
   ```
   - DATETIME 타입을 TIMESTAMP로 변환한 후 EXTRACT 함수로 시간 값 추출.

#### 요약
- 연도, 월, 일: DATE로 처리.
- 시간(HOUR, MINUTE, SECOND): TIMESTAMP로 변환 후 EXTRACT 사용.

---

### 2. 계층형 데이터와 계층형 쿼리

#### 계층형 데이터
- 동일 테이블 내에 계층적으로 상위와 하위 데이터가 포함된 구조.
  - 예: 조직도, 카테고리, 댓글 트리 등.

#### 계층형 쿼리 기본 구조
```sql
SELECT *
  FROM SAMPLE_TABLE
 START WITH PARENT_ID IS NULL
CONNECT BY PRIOR CHILD_ID = PARENT_ID
 ORDER SIBLINGS BY SORT_COLUMN;
```

#### 주요 키워드
- **START WITH**: 최상위 계층 조건 지정.
- **CONNECT BY**: 부모-자식 관계 설정.
- **PRIOR**: 상위 데이터를 참조.
- **ORDER SIBLINGS BY**: 동일 계층의 데이터 정렬.

---

### 3. EXISTS vs IN

#### EXISTS 함수
- **정의**: 서브쿼리의 결과가 존재하는지 확인.
- **특징**:
  - () 안에는 서브쿼리만 포함 가능.
  - 결과가 존재하면 TRUE 반환.

#### IN 함수
- **정의**: 특정 값이 서브쿼리나 리스트에 포함되는지 확인.
- **특징**:
  - () 안에 특정 값 또는 서브쿼리 포함 가능.
  - 결과가 NULL이면 FALSE 반환.

#### 차이점 비교
| 특징       | EXISTS                          | IN                              |
| ---------- | ------------------------------- | ------------------------------- |
| 처리 순서  | 메인 쿼리 → 서브쿼리           | 서브쿼리 → 메인 쿼리           |
| NULL 처리  | NULL도 TRUE로 처리              | NULL은 FALSE로 처리             |
| 성능       | 조건이 복잡할수록 유리           | 데이터 양이 적을수록 유리        |

#### 예제
1. **EXISTS 사용**
   ```sql
   SELECT *
     FROM SAMPLE1 s1
    WHERE EXISTS (
            SELECT * FROM SAMPLE2 s2 WHERE s1.name = s2.name
          );
   ```
2. **IN 사용**
   ```sql
   SELECT *
     FROM SAMPLE1 s1
    WHERE s1.name IN (
            SELECT name FROM SAMPLE2
          );
   ```

---

## 깨달은 것

1. **EXTRACT와 데이터 타입 변환**:
   - DATETIME에서 시간을 추출하려면 TIMESTAMP로 변환해야 함.
2. **계층형 데이터 관리**:
   - START WITH와 CONNECT BY를 활용하여 계층형 데이터를 쉽게 조회 가능.
3. **EXISTS와 IN의 활용**:
   - EXISTS는 조건 존재 여부 확인에, IN은 값 포함 여부 확인에 적합함.


[2024-02-06]
