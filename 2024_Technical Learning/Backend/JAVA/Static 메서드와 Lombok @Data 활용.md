- [x] Static Context 문제 해결 및 DTO와 Map 비교

# Static Context 문제 해결 및 DTO와 Map 비교 사례

## 소개
Java에서 Static Context 문제를 해결한 경험과 DTO(Data Transfer Object)와 Map의 차이를 비교한 내용을 정리함.
이를 통해 Static Context 오류를 방지하는 방법과 데이터 전송 방식 선택 시 고려할 점을 공유함.

## 주요 학습 내용

### 1. Static Context 문제

#### 문제 상황
- **에러 메시지**:
  ```
  java.lang.ClassCastException: class com.repure.purewithme.domain.user.entity.User cannot be cast to class com.repure.purewithme.domain.user.entity.UserProfile
  ```
- **발생 원인**:
  - 정적(static) 메서드에서 비-static 메서드를 호출하려고 시도한 경우 발생.
  - `static` 메서드는 클래스 레벨에서 호출되므로 인스턴스 없이 동작함. 반면, 비-static 메서드는 특정 객체의 인스턴스를 통해 호출되어야 함.

#### 문제 코드 (수정 전)
```java
// Static Context 문제 예시: 수정 전
public class UserService {
    public List<Object> getUserResult(String email) {
        // 비즈니스 로직 수행
        return new ArrayList<>();
    }
}

public class Main {
    public static void main(String[] args) {
        // 정적 메서드처럼 호출하려다 발생한 오류
        List<Object> results = UserService.getUserResult("example@example.com"); // Error 발생
    }
}
```

### 해결 방법

#### 1. 인스턴스를 사용하여 호출
- `UserService` 인스턴스를 통해 비-static 메서드를 호출함.

#### 수정 코드 (수정 후)
```java
// 수정 후: 인스턴스를 통해 호출
public class UserService {
    public List<Object> getUserResult(String email) {
        // 비즈니스 로직 수행
        return new ArrayList<>();
    }
}

public class Main {
    public static void main(String[] args) {
        // 인스턴스를 생성하여 호출
        UserService userService = new UserService();
        List<Object> results = userService.getUserResult("example@example.com"); // 정상 호출
    }
}
```

#### 2. 메서드를 Static으로 변경
- `getUserResult` 메서드를 `static`으로 선언하여 정적 메서드로 사용.
- 하지만 일반적으로 비즈니스 로직을 포함한 메서드는 인스턴스 메서드로 사용하는 것이 권장됨.

---

### 2. @Data 어노테이션

#### 정의
- `@Data`는 Lombok 라이브러리에서 제공하는 어노테이션으로, **Java 클래스에 반복되는 코드를 자동으로 생성**함.
- 이 어노테이션은 클래스의 필드에 대해 다음 기능을 포함:

#### 주요 기능
1. **Getter 메서드 자동 생성**:
   - 클래스의 모든 필드에 대해 `getter` 메서드를 생성.
2. **Setter 메서드 자동 생성**:
   - 클래스의 모든 비-`final` 필드에 대해 `setter` 메서드를 생성.
3. **`toString()` 메서드 자동 생성**:
   - 객체의 모든 필드 값을 포함한 `toString()` 메서드를 자동 생성.
4. **`equals()` 및 `hashCode()` 메서드 생성**:
   - 객체 비교와 해시 코드를 위한 메서드를 자동 생성.
5. **`@RequiredArgsConstructor` 포함**:
   - `final` 필드나 `@NonNull` 필드에 대한 생성자를 자동 생성.

#### 예시 코드
```java
import lombok.Data;

@Data
public class User {
    private Long id;
    private String name;
    private String email;
}

// 위 클래스는 아래와 같은 메서드를 자동으로 생성함:
// - getId(), getName(), getEmail()
// - setId(Long id), setName(String name), setEmail(String email)
// - toString(), equals(), hashCode()
```

#### 장점
1. **코드 간결화**:
   - 반복되는 코드 작성을 줄여 코드 가독성을 높임.
2. **유지보수 용이**:
   - 필드 추가/변경 시 관련 메서드 생성이 자동화되어 생산성 향상.

#### 주의점
- 모든 필드에 대해 `getter`와 `setter`가 생성되므로, 민감한 데이터는 별도로 관리 필요.
- Lombok 라이브러리를 사용하려면 프로젝트에 의존성을 추가해야 하며, IDE(예: IntelliJ, Eclipse)에서 플러그인을 활성화해야 함.

---

### 3. DTO와 Map 비교

#### DTO(Data Transfer Object)
- **정의**: 클라이언트와 서버 간 데이터 교환 시 명확한 데이터 구조를 제공하는 객체.

##### 장점
1. **명확한 데이터 구조**: 필드와 데이터 타입이 명확히 정의되어 있어 코드 가독성이 높음.
2. **타입 안정성**: 컴파일 시점에 오류를 검출 가능.
3. **유지보수 용이**: 데이터 구조 변경 시 DTO 클래스만 수정하면 됨.
4. **데이터 보호**: 필요한 필드만 포함할 수 있어 민감한 정보를 보호 가능.

##### 단점
1. **클래스 파일 증가**: DTO 생성 시 클래스 파일이 많아질 수 있음.

#### Map
- **정의**: 키-값 구조로 데이터를 저장하며, DTO 없이 데이터를 전송 가능.

##### 장점
1. **유연성**: DTO를 생성하지 않아도 다양한 데이터를 담아 즉시 전송 가능.
2. **간단한 데이터 구조**: 임시적인 데이터 전송에 적합.

##### 단점
1. **타입 안정성 부족**: 컴파일 시 타입 검증이 불가능하며, 런타임 오류 발생 가능.
2. **코드 가독성 저하**: 키 값이 명확하지 않으면 코드 가독성이 떨어짐.
3. **유지보수 어려움**: 데이터 구조 변경 시 코드 전반에 영향을 미칠 수 있음.

---

### 결론
- **DTO 권장**:
  - 데이터가 구조화되고 명확한 의미가 필요한 경우.
  - 타입 안정성과 유지보수 용이성을 제공.
- **Map 사용 가능**:
  - 간단하고 임시적인 데이터 전송이 필요한 경우.
  - 단, 장기적인 프로젝트에서는 유지보수가 어려워질 수 있음.

---

## 깨달은 것

1. **Static Context 문제 해결**:
   - 비-static 메서드는 인스턴스를 통해 호출해야 함.
   - Static 메서드는 신중히 사용해야 하며, 비즈니스 로직에는 적합하지 않을 수 있음.
2. **DTO의 장점 이해**:
   - 명확한 데이터 구조와 타입 안정성을 제공하여 코드 품질을 높임.
3. **Map의 사용 범위**:
   - 유연하지만 타입 안정성이 부족하므로 적절한 상황에서만 사용해야 함.
4. **Lombok의 효율성**:
   - `@Data` 어노테이션은 반복 코드를 줄이고 가독성을 향상시켜 생산성을 높임.

[2024-11-13]
