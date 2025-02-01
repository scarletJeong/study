# JPA: @OneToMany vs @ManyToOne 관계

## 소개
JPA에서 `@OneToMany`와 `@ManyToOne` 관계를 설정할 때의 차이점과 올바른 사용법을 정리함.

## 주요 학습 내용

### 1. `@OneToMany` vs `@ManyToOne`
- **`@OneToMany`**: 하나의 엔터티가 여러 개의 하위 엔터티를 참조하는 경우 사용. (**"하나-다수" 관계**)
  - 예시: `User`가 여러 개의 `CartEntity`를 참조할 때.
  - 예시: `ProductEntity`가 여러 개의 `CartEntity`를 참조할 때.

- **`@ManyToOne`**: 여러 엔터티가 하나의 상위 엔터티를 참조하는 경우 사용. (**"다수-하나" 관계**)
  - 예시: `CartEntity`가 하나의 `User`를 참조할 때.
  - 예시: `CartEntity`가 하나의 `ProductEntity`를 참조할 때.

<br>
 
### 2. `@OneToMany` 남용 시의 문제점
- **`@OneToMany`를 남용하면 불필요한 테이블 관계가 생성될 수 있음**. 
  - 한 엔터티가 여러 개의 하위 엔터티를 참조할 때, 지나치게 많은 `@OneToMany` 관계를 사용하면 데이터베이스에서 관리가 복잡해질 수 있음.
- **`@ManyToOne`을 적절히 활용하여 관계를 명확히 설정하는 것이 중요함**.
  - `@ManyToOne`을 사용하면 엔터티 간의 관계를 간결하게 유지하면서도, 데이터베이스 설계를 효율적으로 관리할 수 있음.

### 3. `@OneToMany`와 `@ManyToOne` 관계의 핵심 역할
**1. Entity 간의 관계 표현**
**2. 데이터 무결성 및 일관성 유지**
  - 관계를 명확히 정의하면 데이터 무결성을 유지할 수 있음.
    - CartEntity가 User와 ProductEntity에 종속된다는 것을 @ManyToOne 관계로 정의하면, 데이터베이스에서 외래 키를 통해 연관된 데이터를 일관되게 유지 가능.

**3. 쿼리 성능 최적화**
  - @ManyToOne 관계를 적절히 설정하면 **조인 쿼리 성능을 최적화**할 수 있음.

**4. 데이터베이스 설계 간결화**
  - @OneToMany와 @ManyToOne 관계는 복잡한 비즈니스 로직을 단순화하고, 효율적인 데이터베이스 설계를 가능하게 함.

**5. 데이터베이스 정규화**
  - @OneToMany와 @ManyToOne 관계를 통해 **데이터 중복을 최소화하고 정규화를 실현**할 수 있음.
    - CartEntity가 User와 ProductEntity에 대해 @ManyToOne 관계를 가지면, User와 ProductEntity 테이블은 각각 한 번만 존재하며 CartEntity에서 외래 키로 참조하는 구조가 됨.


<br>

### 4. 사용 사례
`CartEntity`와 `User`, `ProductEntity` 간의 관계를 예시.

```java
@Entity
public class CartEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne
    @JoinColumn(name = "user_id")
    private User user;
    
    @ManyToOne
    @JoinColumn(name = "product_id")
    private ProductEntity product;
}
```
- CartEntity는 User와 ProductEntity를 각각 @ManyToOne 관계로 참조하고 있음.
- @JoinColumn은 외래 키(Foreign Key)를 지정하며, 이 필드를 기준으로 두 엔터티 간의 관계를 정의함.

<br>

---

## 깨달은 것
1. **CartEntity**는 @ManyToOne 관계를 통해 User와 ProductEntity에 종속되므로, 관계가 명확하게 정의되고 쿼리 성능을 최적화할 수 있음.
2. 관계를 잘 설정하면 데이터 무결성 유지, 쿼리 성능 최적화, 정규화가 가능하고, 데이터베이스 설계를 간결하게 유지할 수 있음.
