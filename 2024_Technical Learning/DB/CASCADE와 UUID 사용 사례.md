
- [x] JavaScript vs JSP vs HTML
- [x] CASCADE 사용 이유 > 사용자탈퇴구현
- [x] UUID 사용이유
- [ ] 도메인 설정

- [x] JavaScript vs JSP vs HTML, CASCADE 사용 이유, UUID 사용 이유

# CASCADE와 UUID 활용 사례

## 소개
데이터베이스에서 CASCADE와 UUID를 사용하는 이유와 이를 활용한 설계 사례를 공유함.

## 주요 학습 내용


### 1. CASCADE

#### 정의
- **CASCADE**는 데이터베이스에서 외래 키로 연결된 테이블 간의 작업(DELETE, UPDATE)을 자동으로 처리하는 옵션.
  - `ON DELETE CASCADE`:
    - 부모 테이블의 데이터 삭제 시, 자식 테이블의 데이터도 자동 삭제.
  - `ON UPDATE CASCADE`:
    - 부모 테이블의 데이터 변경 시, 자식 테이블의 데이터도 자동 업데이트.

#### 사용 이유
1. **참조 무결성 유지**:
   - 부모-자식 관계의 데이터 일관성을 보장.
2. **수작업 감소**:
   - 외래 키로 연결된 데이터 관리 시 작업량 절감.
3. **데이터 일관성**:
   - 데이터 삭제 또는 변경 시 연쇄적인 무결성 유지.

#### 사용하지 않는 이유
1. **데이터 관리 제어권 부족**:
   - 자동 삭제로 의도치 않은 데이터 손실 가능성.
2. **논리적 삭제 선호**:
   - 데이터 복구 가능성 및 이력 추적.
3. **DB 퍼포먼스**:
   - 대규모 데이터 삭제 또는 업데이트 시 성능 저하.

#### 예시 (CASCADE 사용 설계)
```sql
CREATE TABLE User (
    id UUID PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(100)
);

CREATE TABLE UserProfile (
    id UUID PRIMARY KEY,
    user_id UUID REFERENCES User(id) ON DELETE CASCADE,
    height FLOAT,
    weight FLOAT
);
```

---

### 2. UUID 사용 이유

#### 정의
- **UUID (Universally Unique Identifier)**는 전역적으로 고유한 식별자.

#### 사용 이유
1. **데이터 무결성**:
   - 고유 식별자로 데이터 충돌 방지.
   - 여러 데이터베이스 병합 시 ID 충돌 가능성 최소화.
2. **데이터베이스 독립성**:
   - 분산 시스템 간 데이터 이동 또는 병합 시 유용.
3. **보안**:
   - 난수 기반으로 예측 불가능한 식별자 제공.
4. **효율성**:
   - 고정된 크기(16바이트)로 인덱싱과 검색 효율 향상.

#### 예시 (UUID 사용 설계)
```sql
CREATE TABLE User (
    id UUID PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(100)
);

CREATE TABLE UserProfile (
    id UUID PRIMARY KEY,
    user_id UUID REFERENCES User(id),
    height FLOAT,
    weight FLOAT
);
```

---

## 깨달은 것

1. **CASCADE 사용 시 주의점**:
   - 데이터 관리 자동화의 장점과 함께 데이터 손실 가능성도 고려해야 함.
   - 논리적 삭제의 장점과 활용 가능성을 이해함.
2. **UUID의 유용성**:
   - 데이터베이스 병합과 보안성 측면에서 UUID의 강점을 확인함.
   - 시스템 간 데이터 충돌 방지를 위한 효율적인 방법임.

[2024-11-19]
