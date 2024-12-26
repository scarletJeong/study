- [x] 문자열 숫자 검증: Double.parseDouble(), String.matches(), Character.isDigit()

# 문자열 숫자 검증 사례와 방법

## 소개
항공권 예약 시스템에서 PNR(항공사 예약 번호) 데이터를 처리하는 과정에서, 숫자인지 검증해야 하는 상황이 발생함. 이 문서는 Java를 활용한 문자열 숫자 검증 방법과 각 방법의 장단점을 정리함.

## 주요 학습 내용

### 1. 문제 상황
- **PNR 데이터 문제**:
  - 항공사에서 제공하는 PNR은 알파벳과 숫자가 혼합된 문자열로 제공되지만, 일부 GDS(Global Distribution System)에서는 숫자만으로 구성된 예약 번호가 들어오는 경우가 있음.
  - 잘못된 숫자 검증 로직(`Double.parseDouble()`)으로 인해 문자열이 숫자로 잘못 처리되는 문제 발생.

---

### 2. 문자열 숫자 검증 방법

#### 1. Double.parseDouble() 메서드 활용
- **코드**:
  ```java
  public boolean isNumeric(String str) {
      try {
          Double.parseDouble(str);
      } catch (NumberFormatException e) {
          return false;
      }
      return true;
  }
  ```
- **장점**:
  - 간단하고 빠르게 숫자 여부를 확인 가능.
- **단점**:
  - D, E 등의 문자가 포함된 경우 지수 형태로 해석되며 잘못된 결과 반환.

#### 2. String.matches() 메서드 활용
- **코드**:
  ```java
  public static boolean isNumeric(String str) {
      return str.matches("[+-]?\\d*(\\.\\d+)?");
  }
  ```
- **장점**:
  - 정규식을 활용하여 세밀하게 숫자 형식을 정의 가능.
- **단점**:
  - 정규식 작성의 복잡성과 성능 이슈 발생 가능.

#### 3. Character.isDigit() 메서드 활용
- **Java 8 미만**:
  ```java
  public boolean isNumeric(String str) {
      boolean result = true;
      for (char c : str.toCharArray()) {
          if (!Character.isDigit(c)) {
              result = false;
              break;
          }
      }
      return result;
  }
  ```

- **Java 8 이상**:
  ```java
  public static boolean isNumeric(String str) {
      return str.chars().allMatch(Character::isDigit);
  }
  ```
- **장점**:
  - 문자 단위로 정확히 검증 가능.
  - Java 8 이상에서는 스트림 API를 사용하여 간결하게 작성 가능.
- **단점**:
  - 숫자 이외의 문자 조합이나 지수 표현은 처리하지 못함.

---

### 3. 예제 시나리오
```java
public class SimpleTesting {
    public static void main(String[] args) {
        String str = "123";
        System.out.println(str.matches("[+-]?\\d*(\\.\\d+)?")); // true

        str = "121xy";
        System.out.println(str.matches("[+-]?\\d*(\\.\\d+)?")); // false

        str = "0x234";
        System.out.println(str.matches("[+-]?\\d*(\\.\\d+)?")); // false
    }
}
```

---

## 깨달은 것

1. **Double.parseDouble()의 한계**:
   - 지수 형태(D, E 등)를 잘못 처리할 가능성이 있으므로 신중히 사용해야 함.
2. **String.matches() 활용**:
   - 정규식을 통해 숫자 여부를 유연하게 판단 가능하지만, 성능에 주의해야 함.
3. **Character.isDigit()의 강점**:
   - 문자 단위로 정확한 숫자 검증 가능하며, Java 8 이상의 스트림 API로 간결하게 작성 가능.

적절한 검증 방법을 선택하여 정확성과 성능을 균형 있게 고려한 데이터 처리가 가능함.



[2024-02-22]

