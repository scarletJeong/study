
- [x] 오라클 변수 선언 대입 연산자 :=

# 오라클 변수 선언과 대입 연산자 :=

## 소개
오라클 데이터베이스에서 변수 선언 및 초기화 시 사용하는 `:=` 연산자에 대해 정리함. 이를 통해 MySQL과 오라클 간 변수 선언 방식의 차이를 이해하고, 오라클의 고유 문법을 익힘.

## 주요 학습 내용

### 1. `:=` 대입 연산자

#### 정의
- `:=`는 오라클 PL/SQL에서 변수에 값을 할당하는 대입 연산자임.
- MySQL의 `=`와 동일한 역할을 수행하지만, 오라클의 PL/SQL에서만 사용 가능.

#### 사용 예시
1. **변수 선언과 초기화**
   ```sql
   DECLARE
       a NUMBER;
   BEGIN
       a := 10; -- 변수 a에 10을 대입
       DBMS_OUTPUT.PUT_LINE('Value of a: ' || a);
   END;
   ```

2. **비교: MySQL과 오라클**
   - MySQL:
     ```sql
     SET @a = 10; -- 변수 선언 및 초기화
     SELECT @a;
     ```
   - 오라클:
     ```sql
     DECLARE
         a NUMBER;
     BEGIN
         a := 10; -- 변수 선언 및 초기화
         DBMS_OUTPUT.PUT_LINE(a);
     END;
     ```

---

### 2. PL/SQL의 주요 특징

1. **DECLARE 섹션**:
   - 변수와 상수를 선언.

2. **BEGIN 섹션**:
   - 선언된 변수로 연산 수행.

3. **DBMS_OUTPUT.PUT_LINE()**:
   - 변수의 값을 출력.

#### 코드 흐름 예시
```sql
DECLARE
    my_var NUMBER;
BEGIN
    my_var := 100;
    DBMS_OUTPUT.PUT_LINE('Value: ' || my_var);
END;
```

---

## 깨달은 것

1. **오라클과 MySQL의 차이점**:
   - 오라클 PL/SQL은 `:=` 연산자를 사용하여 변수를 선언 및 초기화함.
   - MySQL은 `=`를 사용하여 동일한 작업을 수행함.
2. **PL/SQL의 구조 이해**:
   - `DECLARE`, `BEGIN`, `END`로 구성된 블록 구조를 통해 가독성과 유지보수성을 높임.
3. **오라클 고유 문법 숙지**:
   - PL/SQL에서의 문법 차이를 명확히 이해하여 효율적으로 데이터베이스 작업을 수행 가능.



[2024-02-19]



출처: [https://gocoder.tistory.com/2563](https://gocoder.tistory.com/2563) [고코더 IT Express:티스토리]
