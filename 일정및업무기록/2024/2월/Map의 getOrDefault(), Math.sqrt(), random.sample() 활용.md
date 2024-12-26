- [x] Map의 getOrDefault(), Math.sqrt(), random.sample() 활용

# Map의 getOrDefault(), Math.sqrt(), random.sample() 활용 사례

## 소개
`Map.getOrDefault()`, `Math.sqrt()`, `random.sample()` 함수의 사용법과 실제 활용 사례를 정리함. 이를 통해 Java와 Python에서 데이터 검색, 수학 계산, 랜덤 추출을 효율적으로 수행할 수 있음.

## 주요 학습 내용

### 1. Map의 getOrDefault()
- **정의**:
  - 찾는 키가 존재하면 해당 키의 값을 반환하고, 존재하지 않으면 기본 값을 반환하는 메서드.
- **사용법**:
  ```java
  V getOrDefault(Object key, V defaultValue);
  ```
- **매개변수**:
  - `key`: 찾고자 하는 요소의 키.
  - `defaultValue`: 키가 존재하지 않을 경우 반환되는 기본 값.
- **반환값**:
  - 지정된 키가 존재하면 해당 키에 매핑된 값을 반환.
  - 존재하지 않을 경우 기본 값(`defaultValue`) 반환.

#### 예시
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("A", 100);
        map.put("B", 200);

        System.out.println(map.getOrDefault("A", 0)); // 출력: 100
        System.out.println(map.getOrDefault("C", 0)); // 출력: 0
    }
}
```

---

### 2. Math.sqrt() 활용
- **정의**:
  - 제곱근 계산을 위한 Java `Math` 클래스의 메서드.
- **특징**:
  - √N 이하의 자연수로 나누는 방식으로 소수 판별 시 활용 가능.
  - 시간 복잡도: O(√N).
- **원리**:
  - 숫자의 약수들은 곱셈 연산에서 대칭적으로 나타남.

#### 예시
```java
public class Main {
    public static void main(String[] args) {
        int num = 29;
        boolean isPrime = true;

        for (int i = 2; i <= Math.sqrt(num); i++) {
            if (num % i == 0) {
                isPrime = false;
                break;
            }
        }

        System.out.println(num + " is prime: " + isPrime); // 출력: 29 is prime: true
    }
}
```

---

### 3. random.sample() 활용 (Python)
- **정의**:
  - 리스트에서 중복 없이 N개의 요소를 랜덤으로 추출.
- **사용법**:
  ```python
  random.sample(list, N)
  ```
- **매개변수**:
  - `list`: 요소를 추출할 대상 리스트.
  - `N`: 랜덤으로 추출할 요소의 개수.
- **반환값**:
  - 중복되지 않은 N개의 랜덤 요소로 구성된 리스트.

#### 예시
```python
import random

my_list = [1, 2, 3, 4, 5]
random_elements = random.sample(my_list, 3)
print(random_elements)  # 결과 예: [5, 3, 1]
```

---

## 깨달은 것

1. **getOrDefault() 활용**:
   - 키가 없는 경우에도 기본 값을 반환하여 null 검사를 줄일 수 있음.
2. **Math.sqrt() 활용**:
   - 소수 판별 및 수학적 계산에서 성능을 최적화할 수 있음.
3. **random.sample() 활용**:
   - 리스트에서 중복 없이 랜덤 추출이 필요할 때 간편하게 사용 가능.

이 함수들은 효율적인 데이터 처리와 연산을 위한 강력한 도구로 활용될 수 있음.


[2024-02-21] 


출처: [https://splayer.tistory.com/25](https://splayer.tistory.com/25) [S Player:티스토리]
