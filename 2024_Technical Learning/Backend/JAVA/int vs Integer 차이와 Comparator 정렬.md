- [x] int와 Integer 차이 및 Comparator 사용

# int와 Integer 차이 및 Comparator 사용 사례

## 소개
`int`와 `Integer`의 차이점을 이해하고, Java에서 Comparator를 사용하는 방법을 정리함. 이를 통해 기본 자료형과 래퍼 클래스의 차이를 명확히 이해하고 효율적으로 데이터를 정렬할 수 있음.

## 주요 학습 내용

### 1. int와 Integer 차이

#### int (기본 자료형)
- **특징**:
  - Primitive type으로, 값 자체를 저장.
  - 메모리 효율이 좋고, 비교 연산 시 속도가 빠름.
- **제약**:
  - Java의 제네릭에서 사용 불가 (제네릭은 객체를 다룸).

#### Integer (래퍼 클래스)
- **특징**:
  - `java.lang.Integer` 클래스의 객체.
  - `int` 값을 객체로 감싸며, null 값을 허용.
  - Java의 컬렉션 프레임워크나 제네릭에서 사용 가능.

#### 차이점 비교
| 특성              | int             | Integer         |
| ----------------- | --------------- | --------------- |
| 타입              | Primitive type  | Reference type  |
| 메모리 사용       | Stack           | Heap            |
| null 허용 여부    | 불가능           | 가능             |
| 제네릭에서 사용   | 불가능           | 가능             |
| 속도              | 빠름             | 느림             |

---

### 2. Comparator에서 Integer 사용

#### 문제 상황
- **Comparator**는 제네릭 타입을 사용하므로, 기본 자료형인 `int`는 사용할 수 없음.
- 대신, `Integer`를 사용하여 데이터를 정렬해야 함.

#### 해결 방법
1. **Integer 배열 선언 및 Comparator 사용**
   ```java
   import java.util.*;

   public class ComparatorExample {
       public static void main(String[] args) {
           Integer[] numbers = {3, 1, 4, 1, 5};

           Arrays.sort(numbers, new Comparator<Integer>() {
               @Override
               public int compare(Integer o1, Integer o2) {
                   return o2 - o1; // 내림차순 정렬
               }
           });

           System.out.println(Arrays.toString(numbers));
       }
   }
   ```

2. **Lambda 사용**
   ```java
   import java.util.*;

   public class ComparatorLambda {
       public static void main(String[] args) {
           Integer[] numbers = {3, 1, 4, 1, 5};

           Arrays.sort(numbers, (o1, o2) -> o2 - o1); // 내림차순 정렬

           System.out.println(Arrays.toString(numbers));
       }
   }
   ```

---

### 3. toCharArray()와 .split()의 차이

#### 문제 상황
- `toCharArray()`는 `char[]` 배열을 반환하며, 기본 자료형으로 구성됨.
- `.split()`은 문자열 배열(`String[]`)을 반환.

#### 원인
- `toCharArray()` 결과는 `Comparator`의 제네릭 타입에 맞지 않음.
- `.split()`은 `String` 객체를 반환하므로 `Comparator`에서 사용 가능.

#### 코드 비교
1. **toCharArray() 사용 불가**
   ```java
   char[] chars = "hello".toCharArray();
   // Arrays.sort(chars, new Comparator<char>()); -> 컴파일 에러
   ```

2. **.split() 사용 가능**
   ```java
   String[] parts = "h,e,l,l,o".split(",");

   Arrays.sort(parts, (a, b) -> b.compareTo(a)); // 내림차순 정렬
   System.out.println(Arrays.toString(parts));
   ```

---

## 깨달은 것

1. **int와 Integer의 차이**:
   - `int`는 메모리 효율이 높고 빠르지만, 제네릭에서 사용할 수 없음.
   - `Integer`는 객체로서의 유연성을 제공하며, null 처리가 가능함.
2. **Comparator 활용**:
   - 제네릭을 사용하는 Comparator는 `Integer`와 같은 래퍼 클래스를 요구함.
3. **toCharArray와 split의 차이**:
   - `toCharArray()`는 기본 자료형을 반환하여 Comparator와 호환되지 않음.
   - `.split()`은 문자열 객체를 반환하여 Comparator와 호환 가능함.

[2024-02-12]
