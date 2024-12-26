- [x] SQL에서 ROWNUM과 정렬된 결과 처리

# Oracle: ROWNUM과 정렬된 결과 처리

## 소개
Oracle SQL에서 `ROWNUM`은 결과 집합의 행 순서를 나타내는 의사 열로, 쿼리 결과를 제한하는 데 자주 사용됨. 그러나 `ROWNUM`은 정렬된 결과에 직접 영향을 미치지 않으므로, 이를 효과적으로 활용하려면 서브쿼리와 같은 추가적인 작업이 필요함.

---

## 주요 학습 내용

### 1. ROWNUM의 특징
- **정의**:
  - `ROWNUM`은 결과 집합의 각 행에 부여되는 임시 번호.
- **처리 순서**:
  - 정렬(`ORDER BY`) 작업 이전에 부여됨.
  - 따라서 정렬된 결과에서 `ROWNUM`을 사용하려면 서브쿼리가 필요함.

#### 일반적인 문제
```sql
SELECT *
FROM fish_info
WHERE ROWNUM <= 10
ORDER BY length DESC;
```
- **문제점**:
  - `ROWNUM`이 먼저 처리되므로, 정렬되지 않은 상위 10개 행만 반환됨.

---

### 2. 서브쿼리를 사용한 정렬 후 ROWNUM 처리
#### 해결 방법
- 서브쿼리를 사용하여 정렬된 결과 집합을 먼저 생성한 후, `ROWNUM`을 적용.

#### 수정된 쿼리
```sql
SELECT *
FROM (
    SELECT *
    FROM fish_info
    WHERE length IS NOT NULL
    ORDER BY length DESC
)
WHERE ROWNUM <= 10;
```
- **설명**:
  - 내부 쿼리에서 데이터를 정렬한 후, 외부 쿼리에서 `ROWNUM`으로 상위 10개 행 제한.

---

### 3. Oracle 12c 이상에서 FETCH FIRST 사용
- Oracle 12c부터는 `FETCH FIRST` 구문으로 간단하게 정렬된 결과 집합에서 상위 N개의 행을 가져올 수 있음.

#### 예제
```sql
WITH fish_len AS (
    SELECT id, length
    FROM fish_info
    WHERE length IS NOT NULL
    ORDER BY length DESC, id
)
SELECT *
FROM fish_len
FETCH FIRST 10 ROWS ONLY;
```
- **장점**:
  - 서브쿼리 없이도 간단하게 작성 가능.
  - 코드 가독성이 높아짐.

---

### 4. 윈도우 함수로 대체하기
#### ROW_NUMBER 함수 사용
- 정렬된 결과에서 행 번호를 부여하고 조건에 따라 필터링 가능.

#### 예제
```sql
WITH fish_rank AS (
    SELECT id, length, ROW_NUMBER() OVER (ORDER BY length DESC, id) AS rnk
    FROM fish_info
    WHERE length IS NOT NULL
)
SELECT *
FROM fish_rank
WHERE rnk <= 10;
```
- **장점**:
  - 정렬된 순서를 유지하면서 행 번호를 부여.
  - 다중 조건 정렬과 함께 사용 가능.

---

## 깨달은 것
1. **ROWNUM의 한계**:
   - 정렬된 결과를 직접 처리하지 못하므로 서브쿼리 또는 다른 방법이 필요함.
2. **Oracle 12c 기능 활용**:
   - `FETCH FIRST` 구문으로 간단하게 상위 N개의 결과를 가져올 수 있음.
3. **윈도우 함수의 유용성**:
   - `ROW_NUMBER`와 같은 윈도우 함수를 활용하면 복잡한 정렬 및 필터링 작업을 간소화 가능.


