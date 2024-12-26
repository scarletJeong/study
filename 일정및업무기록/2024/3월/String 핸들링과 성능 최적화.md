- [x] String 핸들링 및 성능 최적화

# String 핸들링과 성능 최적화

## 소개
Java에서 문자열 처리는 코드의 가독성과 성능에 큰 영향을 미침. 이 문서에서는 문자열 변환, 조건문 통합, 정렬 최적화, 그리고 문자열 타입 선택을 통해 성능을 개선하는 방법을 정리함.

## 주요 학습 내용

### 1. char를 Integer로 바로 변환
- **문제점**:
  ```java
  String Xidx = String.valueOf(X.charAt(i));
  int idx = Integer.parseInt(Xidx);
  ```
  - `char`를 `String`으로 변환한 후 다시 `int`로 변환.
  - 불필요한 변환 단계가 추가되어 성능 저하 발생 가능.

- **해결 방법**:
  ```java
  int idx = Character.getNumericValue(X.charAt(i));
  ```
  - `Character.getNumericValue()`를 사용해 `char`를 직접 `int`로 변환.

---

### 2. 조건문 통합
- **문제점**:
  ```java
  if (Xnum[i] == Ynum[i]) XYnum[i] = Xnum[i];
  else if (Xnum[i] < Ynum[i]) XYnum[i] = Xnum[i];
  ```
  - 동일한 동작을 수행하는 조건문이 여러 번 작성됨.

- **해결 방법**:
  ```java
  if (Xnum[i] <= Ynum[i]) XYnum[i] = Xnum[i];
  ```
  - 조건문을 통합하여 코드 간결성과 가독성 향상.

---

### 3. 정렬 방식 최적화
- **문제점**:
  - 이미 오름차순으로 정렬된 데이터를 내림차순으로 변경하려면 추가 연산이 필요.

- **해결 방법**:
  - 데이터 삽입 시 내림차순으로 정렬되도록 초기 설계를 변경.
  - 예:
    ```java
    PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
    pq.addAll(Arrays.asList(data));
    ```

---

### 4. String 타입 선택
- **문제점**:
  ```java
  str += i;
  ```
  - `String`은 불변(Immutable) 객체로, 새로운 문자열이 생성될 때마다 메모리를 재할당.
  - 반복문에서 `str`을 수정하면 많은 메모리 할당이 발생하여 성능 저하.

- **해결 방법**:
  - 문자열이 자주 변형되는 경우 `StringBuilder` 또는 `StringBuffer` 사용.
  - 예:
    ```java
    StringBuilder sb = new StringBuilder();
    sb.append(i);
    ```

---

## 깨달은 것

1. **데이터 타입에 따른 성능 차이**:
   - 불필요한 변환을 최소화하여 효율적으로 데이터를 처리할 수 있음.
2. **코드 통합의 중요성**:
   - 조건문이나 중복 코드를 통합해 코드 가독성과 유지보수성을 향상시킬 수 있음.
3. **적합한 자료구조와 알고리즘 선택**:
   - 초기 설계를 통해 추가 연산을 줄이고, 성능 최적화를 실현할 수 있음.
4. **문자열 핸들링 최적화**:
   - 불변 객체의 특성을 이해하고 적합한 문자열 클래스(`StringBuilder`/`StringBuffer`)를 활용해야 함.

이러한 최적화 방법을 통해 코드는 가독성과 성능에서 모두 향상될 수 있음.


[2024-03-05]
