
- [x] 코테 - 짝지어 제거하기
- [x] 스택공부
- [x] JPA - entity  맞추기.

- [x] ClassCastException 해결 및 JPA Entity 매핑 사례

# ClassCastException 해결 및 JPA Entity 매핑 사례

## 소개
JPA를 사용하면서 발생한 `ClassCastException` 문제를 분석하고 해결한 사례를 정리함.
이를 통해 엔티티 매핑과 리포지토리 정의의 중요성을 공유함.

## 주요 학습 내용

### 1. ClassCastException 문제 분석

#### 발생 원인
- **에러 메시지**:
  ```
  java.lang.ClassCastException: class com.repure.purewithme.domain.user.entity.User cannot be cast to class com.repure.purewithme.domain.user.entity.UserProfile
  ```
- **원인 분석**:
  - `findByToken` 메서드에서 `UserProfile` 타입으로 데이터를 조회하려 했지만, 실제로는 `User` 타입 객체가 반환됨.
  - 이는 데이터베이스에서 반환된 객체의 타입과 메서드 정의의 반환 타입이 일치하지 않아 발생한 문제임.

#### 문제 코드 (수정 전)
```java
public interface UserProfileRepository extends JpaRepository<UserProfile, Long> {
    Optional<User> findByToken(String token);  // 반환 타입이 Optional<User>
}
```
- `findByToken` 메서드의 반환 타입이 `Optional<User>`로 정의되어 있었음.
- `UserService`에서 반환된 `Optional<User>`를 `Optional<UserProfile>`로 변환하려다 에러 발생.

### 2. 문제 해결

#### 수정 내용
- `findByToken` 메서드의 반환 타입을 `Optional<UserProfile>`로 변경함.

#### 수정 코드 (수정 후)
```java
public interface UserProfileRepository extends JpaRepository<UserProfile, Long> {
    Optional<UserProfile> findByToken(String token);
}
```
- 반환 타입을 `UserProfile`로 수정하여 데이터베이스에서 `UserProfile` 엔티티를 반환하도록 보장함.
- 이를 통해 `ClassCastException` 문제를 해결함.

---

## 깨달은 것

1. **리포지토리 메서드 정의 중요성**:
   - 리포지토리 메서드의 반환 타입은 데이터베이스에서 반환되는 엔티티와 정확히 일치해야 함.
2. **엔티티 매핑의 중요성**:
   - JPA 엔티티의 정확한 정의와 매핑은 데이터 처리 과정에서 발생할 수 있는 타입 관련 문제를 예방함.
3. **테스트의 필요성**:
   - 리포지토리 메서드 정의와 관련된 변경 사항은 단위 테스트를 통해 반드시 검증해야 함.

[2024-11-12]
