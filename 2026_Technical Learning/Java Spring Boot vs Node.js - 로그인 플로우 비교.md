
## 1. 소개

Java(Spring Boot) 기반 로그인 기능을 Node.js로 변환하면서 두 환경의 차이를 정리해보았다.

---

## 2. 전체 요청 처리 흐름 비교

### 2.1 요청 처리 단계 요약

| 단계 | Java Spring Boot | Node.js |
| --- | --- | --- |
| 1 | HTTP 요청 도착 | HTTP 요청 도착 |
| 2 | DispatcherServlet이 요청 수신 | Express.js가 요청 수신 |
| 3 | HandlerMapping으로 Controller 탐색 | 미들웨어 체인 실행 |
| 4 | @RestController 확인 | 라우터 매칭 |
| 5 | @Autowired로 의존성 주입 | require로 의존성 로드 |
| 6 | @PostMapping 메서드 실행 | Controller 함수 호출 |
| 7 | @RequestBody → DTO 변환 | req.body에서 JSON 추출 |
| 8 | Repository 메서드 호출 | Repository 메서드 호출 |
| 9 | JPQL → SQL 변환 | SQL 직접 작성 |
| 10 | 커넥션 풀에서 DB 연결 | 커넥션 풀에서 DB 연결 |
| 11 | ResultSet → Entity 자동 매핑 | 결과 배열 반환 |
| 12 | 비즈니스 로직 실행 | 비즈니스 로직 실행 |
| 13 | ResponseEntity 생성 | res.json() 호출 |
| 14 | 객체 → JSON 자동 변환 | JSON 자동 변환 |
| 15 | HTTP 응답 반환 | HTTP 응답 반환 |

---

## 3. 로그인 요청 상세 흐름

### 3.1 Java Spring Boot

1. 클라이언트  
   - POST /api/auth/login  
   - Body: `{ "email": "...", "password": "..." }`

2. DispatcherServlet  
   - URL 분석
   - HandlerMapping으로 Controller 메서드 탐색

3. Spring 컨테이너  
   - @Autowired로 Repository 주입  
   - @RequestBody로 JSON → DTO 변환

4. Controller  
   - Repository 호출
   - Optional<User> 처리

5. JPA / Hibernate  
   - JPQL → SQL 변환
   - Entity 자동 매핑

6. 비밀번호 검증 및 상태 업데이트

7. ResponseEntity 생성  
   - Jackson이 JSON으로 변환

---

### 3.2 Node.js (Express)

1. 클라이언트  
   - POST /api/auth/login

2. Express 미들웨어  
   - express.json()으로 JSON 파싱
   - req.body에 저장

3. 라우터 매칭  
   - router.post('/login')

4. Controller  
   - req.body 구조 분해
   - Repository 직접 호출

5. Repository  
   - SQL 직접 작성
   - 결과 배열 반환

6. User 객체 수동 생성

7. 비밀번호 검증 및 업데이트

8. res.json()으로 응답 반환

---

## 4. 주요 차이점 정리

### 4.1 프레임워크 구조

- **Java**
  - 어노테이션 기반
  - 런타임에 동작 구성
  - 프록시, AOP 활용

- **Node.js**
  - 함수 기반
  - 명시적 연결
  - 실행 흐름이 코드에 그대로 드러남

---

### 4.2 의존성 주입

- Java  
  - Spring IoC 컨테이너가 Bean 관리
  - 인터페이스 기반 설계

- Node.js  
  - require / module.exports
  - 모듈 캐싱으로 싱글톤 구현

---

### 4.3 데이터베이스 접근

- Java  
  - JPA / Hibernate
  - 자동 매핑
  - Optional로 null 안정성 확보

- Node.js  
  - SQL 직접 작성
  - 수동 매핑
  - null 체크 직접 처리

---

### 4.4 타입 시스템

- Java  
  - 컴파일 타임 타입 체크
  - IDE 자동완성, 리팩토링 강력

- Node.js  
  - 런타임 타입 결정
  - TypeScript 사용 시 보완 가능

---

### 4.5 비동기 처리

- Java  
  - 기본은 동기적 코드
  - 내부적으로 스레드 풀 사용

- Node.js  
  - async / await 필수
  - 이벤트 루프 기반

---

### 4.6 데이터 모델링

- Java  
  - Entity + 어노테이션
  - Getter / Setter 필요

- Node.js  
  - 생성자에서 직접 매핑
  - 자유로운 메서드 구성

---

### 4.7 에러 처리

- Java  
  - checked / unchecked 예외
  - 상세한 스택 트레이스

- Node.js  
  - 모든 에러는 Error 객체
  - stack 자동 포함

---

## 5. 성능과 동시성

| 항목 | Java Spring Boot | Node.js |
| --- | --- | --- |
| 스레드 | 멀티 스레드 | 단일 스레드 |
| 처리 방식 | 요청당 스레드 | 이벤트 루프 |
| 강점 | CPU 집약 작업 | I/O 집약 작업 |
| 단점 | 메모리 사용량 큼 | CPU 작업에 취약 |

---

## 6. 개발 경험 비교

| 항목 | Java Spring Boot | Node.js |
| --- | --- | --- |
| 코드 반영 | 재컴파일 필요 | 즉시 반영 |
| 디버깅 | 강력 | 상대적으로 단순 |
| 학습 곡선 | 높음 | 낮음 |
| 생산성 | 안정적 | 빠름 |

---

## 7. 깨달은 점

- Spring Boot는 **대규모 도메인과 안정성**에 강하다.
- Node.js는 **변화가 잦은 API와 빠른 개발**에 적합하다.
- 추상화가 많을수록 편해지지만, 동작을 이해하기는 어려워진다.
- Node.js는 모든 흐름을 직접 작성해야 하지만, 그만큼 동작이 명확하다.

---

## 작성일
- 2026-02-06
