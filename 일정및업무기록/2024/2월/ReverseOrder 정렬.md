- [x] Collections.reverseOrder(), toCharArray(), Substring 불변성, Long vs Int

# Java 주요 기능 및 데이터 처리 사례

## 소개
Java에서 자주 사용되는 기능인 `Collections.reverseOrder()`, `toCharArray()` 메서드, `substring()` 메서드의 불변성 개념과, Long과 Int의 차이에 대해 정리함. 이를 통해 효율적인 정렬, 문자열 처리, 그리고 적절한 데이터 타입 선택 방법을 학습함.

## 주요 학습 내용

### 1. Collections.reverseOrder()
- **정의**:
  - Java의 `Collections.reverseOrder()` 메서드는 배열이나 리스트를 역순으로 정렬하는 Comparator를 반환함.

#### 사용법
```java
Arrays.sort(array, Collections.reverseOrder());
```
- Arrays.sort() 메서드의 두 번째 파라미터로 `Collections.reverseOrder()`를 전달하면 배열을 내림차순으로 정렬할 수 있음.

#### 주의사항
- **프리미티브 타입 배열은 사용 불가**:
  - `Collections.reverseOrder()`는 `Object`를 상속한 클래스에만 적용 가능.
  - 프리미티브 타입(int, double 등)을 사용할 경우, 래퍼 클래스(Integer, Double 등)로 변환 필요.

#### 예시
```java
Integer[] numbers = {3, 1, 4, 1, 5};
Arrays.sort(numbers, Collections.reverseOrder());
System.out.println(Arrays.toString(numbers));
// 출력: [5, 4, 3, 1, 1]
```

---

### 2. 문자열을 문자 배열로 변환: toCharArray() vs charAt()

#### toCharArray()
- **정의**:
  - `toCharArray()`는 문자열을 문자 배열로 변환하는 String 클래스의 인스턴스 메서드.
- **장점**:
  - 문자열 전체를 문자 배열로 쉽게 변환 가능.
- **예시**:
  ```java
  String vowels = "aeiou";
  char[] vowelArray = vowels.toCharArray();
  System.out.println(Arrays.toString(vowelArray));
  // 출력: [a, e, i, o, u]
  ```

#### charAt()
- **정의**:
  - `charAt()`은 특정 인덱스의 문자를 반환하는 String 클래스의 인스턴스 메서드.
- **장점**:
  - 특정 문자만 선택적으로 처리 가능.
- **예시**:
  ```java
  String vowels = "aeiou";
  char[] vowelArray = new char[vowels.length()];
  for (int i = 0; i < vowels.length(); i++) {
      vowelArray[i] = vowels.charAt(i);
  }
  System.out.println(Arrays.toString(vowelArray));
  // 출력: [a, e, i, o, u]
  ```

---

### 3. Substring() 메서드의 불변성
- **정의**:
  - `String` 클래스의 `substring()` 메서드는 원본 문자열에서 일부 문자열을 잘라내 새로운 문자열을 반환함.
- **불변성(Immutable)**:
  - `String` 클래스는 불변 클래스이며, 메서드 호출 시 원본 문자열은 변경되지 않음.
- **예시**:
  ```java
  String original = "Hello, World!";
  String sub = original.substring(7, 12);
  System.out.println(sub); // 출력: World
  System.out.println(original); // 출력: Hello, World!
  ```
  - 원본 문자열 `original`은 그대로 유지됨.

---

### 4. Long vs Int

#### 문제 상황
- 숫자의 길이에 따라 `long`과 `int` 중 적합한 데이터 타입을 선택해야 하는 경우 발생.

#### 제약 조건
- **int**:
  - 최대 자리수: 10자리 (2,147,483,647까지 표현 가능).
- **long**:
  - 최대 자리수: 19자리 (9,223,372,036,854,775,807까지 표현 가능).

#### 예시
- 문자열 `p`의 최대 길이가 18자리일 경우, `int`는 초과 값을 저장할 수 없으므로 `long`을 사용해야 함.
```java
String p = "123456789123456789";
long value = Long.parseLong(p);
System.out.println(value);
```

---

## 깨달은 것

1. **Collections.reverseOrder() 활용**:
   - 배열을 역순으로 정렬할 때 유용하며, 프리미티브 타입 배열은 래퍼 클래스로 변환 필요.
2. **toCharArray()와 charAt() 차이**:
   - `toCharArray()`는 전체 문자열 변환에, `charAt()`은 특정 문자 접근에 적합.
3. **Substring 불변성**:
   - `String` 클래스의 불변성을 이해하여 안전하게 문자열을 처리 가능.
4. **Long과 Int의 차이**:
   - 데이터 크기에 따라 적합한 타입 선택이 필요하며, 초과값 처리 시 `long` 사용이 필수적임.


[2024-02-13]
