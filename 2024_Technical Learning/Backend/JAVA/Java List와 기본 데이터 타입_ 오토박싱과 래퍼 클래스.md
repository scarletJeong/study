- [x] Java List와 기본 데이터 타입: 오토박싱과 래퍼 클래스

# Java List와 기본 데이터 타입

## 소개
Java에서 `List`와 같은 제네릭 컬렉션은 기본 데이터 타입(`int`, `double` 등)을 직접 사용할 수 없으며, 대신 래퍼 클래스(`Integer`, `Double` 등)를 사용해야 함. 이 문서에서는 Java 제네릭과 오토박싱(auto-boxing) 기능의 원리를 이해하고, 이를 활용하는 방법을 정리함.

## 주요 학습 내용

### 1. 제네릭과 기본 데이터 타입의 제약
- **제네릭의 특성**:
  - Java의 제네릭은 객체 참조만 처리 가능.
  - 기본 데이터 타입은 객체가 아니므로 직접적으로 제네릭에 사용할 수 없음.

- **래퍼 클래스**:
  - 기본 데이터 타입의 래퍼 클래스는 제네릭에서 사용 가능.
  - 예: `int`의 래퍼 클래스는 `Integer`, `double`의 래퍼 클래스는 `Double`.

#### 예시
```java
List<int> list = new ArrayList<>(); // 컴파일 에러 발생
List<Integer> list = new ArrayList<>(); // 올바른 코드
```

---

### 2. 오토박싱과 언박싱
- **오토박싱(auto-boxing)**:
  - 기본 데이터 타입을 해당 래퍼 클래스로 자동 변환.
  - 예: `int` → `Integer`.

- **언박싱(unboxing)**:
  - 래퍼 클래스를 기본 데이터 타입으로 자동 변환.
  - 예: `Integer` → `int`.

#### 동작 원리
- 컴파일러가 자동으로 변환 코드를 삽입하여 처리.
- 개발자가 직접 변환 코드를 작성할 필요가 없음.

#### 예시 코드
```java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        int value = 5;

        // 오토박싱: int → Integer
        list.add(value);

        // 언박싱: Integer → int
        int extractedValue = list.get(0);

        System.out.println("List: " + list); // 출력: List: [5]
        System.out.println("Extracted Value: " + extractedValue); // 출력: Extracted Value: 5
    }
}
```

---

### 3. 활용 사례와 주의점
#### 활용 사례
1. **컬렉션에서 기본 타입 처리**:
   - `List<Integer>`를 사용하여 숫자 목록 저장 및 처리.

2. **스트림 API와 래퍼 클래스**:
   - 스트림에서 기본 타입을 처리하기 위해 `mapToInt`와 같은 메서드 사용.

#### 주의점
1. **성능 문제**:
   - 오토박싱과 언박싱은 추가적인 객체 생성 및 변환 비용이 발생.
   - 대량의 데이터 처리 시 성능 저하 가능.

2. **NullPointerException**:
   - 래퍼 클래스가 `null`을 가질 수 있으므로, 언박싱 시 NPE 발생 가능성 존재.

   ```java
   List<Integer> list = new ArrayList<>();
   list.add(null);

   int value = list.get(0); // NullPointerException 발생
   ```

---

## 깨달은 것

1. **제네릭의 한계와 래퍼 클래스 사용**:
   - Java 제네릭은 객체 참조만 허용하며, 래퍼 클래스를 사용해 기본 타입을 처리.

2. **오토박싱의 편리함**:
   - 코드를 간결하게 작성할 수 있지만, 성능과 안정성에 주의해야 함.

3. **언박싱 주의점**:
   - NPE 방지를 위해 래퍼 클래스를 신중히 관리해야 함.

4. **효율적 데이터 처리를 위한 설계**:
   - 기본 타입 처리가 많을 경우, `IntStream`과 같은 기본 타입 전용 API를 활용하면 성능 향상 가능.


[2024-03-06]
