- [x] String, StringBuilder, StringBuffer 차이

# String, StringBuilder, StringBuffer의 차이

## 소개
Java에서 문자열을 다룰 때 사용하는 주요 클래스인 `String`, `StringBuilder`, `StringBuffer`의 차이를 정리함. 이를 통해 각 클래스의 특징과 적합한 사용 사례를 이해할 수 있음.

## 주요 학습 내용

### 1. String
- **특징**:
  - 불변(Immutable) 객체: String 객체의 값은 변경할 수 없음.
  - 새로운 문자열을 할당하면, 기존 객체가 아닌 새로운 객체가 생성됨.
- **장점**:
  - 변경되지 않는 문자열에 대해 안정적인 사용 가능.
  - String Pool에 저장되어 메모리 사용 최적화.
- **단점**:
  - 문자열 변경 시, 새로운 객체를 생성하므로 성능 저하 및 메모리 낭비 발생.

#### 예시
```java
String str = "Hello";
str = str + " World"; // 새로운 객체가 생성됨
System.out.println(str); // 출력: Hello World
```

---

### 2. StringBuilder
- **특징**:
  - 가변(Mutable) 객체: 문자열 연산 시 동일한 객체 내에서 값을 변경.
  - 단일 스레드 환경에서 효율적.
- **장점**:
  - 문자열 연산이 빠르고, 메모리 낭비가 적음.
  - 문자열 파싱 성능이 우수함.
- **단점**:
  - 멀티 스레드 환경에서는 비추천 (스레드 안정성 부족).

#### 예시
```java
StringBuilder sb = new StringBuilder("Hello");
sb.append(" World");
System.out.println(sb); // 출력: Hello World
```

---

### 3. StringBuffer
- **특징**:
  - 가변(Mutable) 객체: 문자열 연산 시 동일한 객체 내에서 값을 변경.
  - 멀티 스레드 환경에서 안전 (스레드 동기화 지원).
- **장점**:
  - 멀티 스레드 환경에서 안정적으로 동작.
  - 문자열 연산 성능이 개선됨.
- **단점**:
  - 동기화로 인해 단일 스레드 환경에서 성능이 떨어질 수 있음.

#### 예시
```java
StringBuffer sb = new StringBuffer("Hello");
sb.append(" World");
System.out.println(sb); // 출력: Hello World
```

---

### 4. String, StringBuilder, StringBuffer 비교
| 특성              | String            | StringBuilder     | StringBuffer      |
| ----------------- | ----------------- | ----------------- | ----------------- |
| 가변성            | 불변 (Immutable)  | 가변 (Mutable)    | 가변 (Mutable)    |
| 스레드 안정성      | 안전              | 안전하지 않음       | 안전              |
| 성능              | 낮음              | 가장 빠름          | 빠름              |
| 사용 사례          | 변경이 적은 경우  | 단일 스레드 환경    | 멀티 스레드 환경    |

---

## 깨달은 것

1. **String의 불변성**:
   - 메모리 관리와 변경 가능한 데이터 처리에서 불리할 수 있지만, 안정성이 높음.
2. **StringBuilder의 효율성**:
   - 단일 스레드 환경에서 문자열 조작이 빠르고 메모리 사용이 적음.
3. **StringBuffer의 안정성**:
   - 멀티 스레드 환경에서 동기화를 통해 안정적으로 동작함.

적합한 클래스 선택을 통해 메모리와 성능을 최적화할 수 있음.

[2024-02-15]
