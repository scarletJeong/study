- [x] equals()와 hashCode() 차이 및 활용

# equals()와 hashCode() 차이 및 활용 사례

## 소개
Java의 `equals()`와 `hashCode()` 메서드의 차이와 이를 사용하는 이유를 정리함. 이를 통해 객체 비교의 본질을 이해하고, 해시 기반 자료구조(HashSet, HashMap)에서 효율적인 데이터 저장과 검색 방법을 학습함.

## 주요 학습 내용

### 1. equals()와 hashCode() 메서드

#### 공통점
- **Object 클래스의 메서드**:
  - `equals()`와 `hashCode()`는 모든 Java 객체에 기본적으로 제공되는 `Object` 클래스의 메서드.

#### equals()
- **정의**:
  - 객체 내부 값의 일치 여부를 확인하여 `boolean` 값을 반환.
- **기본 동작**:
  - `==` 연산자와 동일한 결과를 반환하며, 참조값(객체의 주소값)이 같은지를 판단.
- **재정의 필요성**:
  - 논리적 동등성을 확인하려면 `equals()`를 오버라이딩하여 내부 값을 비교하도록 수정해야 함.

```java
@Override
public boolean equals(Object obj) {
    if (this == obj) return true;
    if (obj == null || getClass() != obj.getClass()) return false;
    MyClass other = (MyClass) obj;
    return this.value.equals(other.value);
}
```

#### hashCode()
- **정의**:
  - 객체를 구별하는 고유한 정수 값(해시 코드)을 반환.
- **기본 동작**:
  - 객체의 메모리 주소를 기반으로 해시 코드를 생성.
- **사용 목적**:
  - 해시 기반 자료구조(HashSet, HashMap)에서 객체를 효율적으로 저장하고 검색하기 위함.
- **주의**:
  - 해시 충돌이 발생할 수 있으므로, 유일성 보장은 불가능.

```java
@Override
public int hashCode() {
    return Objects.hash(value);
}
```

---

### 2. equals()와 hashCode() 관계

#### 규칙
1. **equals()가 true를 반환하면 hashCode() 값도 반드시 같아야 함.**
2. **hashCode() 값이 같아도 equals()가 true를 보장하지는 않음.**

#### 예시
```java
Object obj1 = new Object();
Object obj2 = new Object();
System.out.println(obj1.equals(obj2)); // false
System.out.println(obj1.hashCode() == obj2것
1. what is method of 'Object'?
2. String 변수를 선언하면서 리터럴을 이용하는것과 `new` 키워드로 선언하는 것의 차이
3. 객체의 주소값,, 메모리 주소.. ㅁ약간 달랐는데 헷갈린다. 
4. https://velog.io/@younghoondoodoom/JAVA-equals%EC%99%80-hashcode    > 여기 이해못하겠다..


[2024-02-14]
