# 사용자 인가 코드 처리 방식 이해

## 소개

OAuth 2.0 프로토콜에서 사용자 인가 코드는 클라이언트 애플리케이션이 리소스 서버에 접근할 수 있도록 인증 과정을 거치는 핵심 요소임.   
인가 코드에 대해 학습하고, 데이터베이스에 저장하는 과정을 정리함.

---

## 주요 학습 내용

### 1. 사용자 인가 코드란?

#### 정의
- 인가 코드는 OAuth 인증 과정에서 클라이언트 애플리케이션이 인증 서버로부터 받는 일회성 코드임.
- 이 코드는 클라이언트 애플리케이션이 사용자 승인을 얻었음을 나타내며, 이를 통해 액세스 토큰을 발급받음.

#### 인가 코드의 주요 목적
- 사용자 인증 및 승인을 검증.
- 클라이언트 애플리케이션이 인증 서버와 안전하게 통신하도록 지원.

---

### 2. 인가 코드의 흐름

#### 요청 및 응답 단계
1. **사용자 인증 요청**:
   - 클라이언트는 인증 서버에 사용자의 인증 및 승인을 요청함.
   - 예:
     ```http
     GET /authorize?response_type=code&client_id=CLIENT_ID&redirect_uri=REDIRECT_URI&state=STATE
     ```

2. **사용자 승인 후 리디렉션**:
   - 사용자 승인이 완료되면 인증 서버는 클라이언트의 리디렉션 URI로 인가 코드와 상태 값을 반환함.
   - 예:
     ```http
     GET /redirect_uri?code=AUTH_CODE&state=STATE
     ```

3. **컨트롤러에서 코드 수신 및 처리**:
   - 클라이언트는 전달받은 인가 코드를 컨트롤러에서 처리함.
   - 예:
     ```java
     @GetMapping("/callback")
     public ResponseEntity<?> handleAuthorizationCode(@RequestParam("code") String code,
                                                      @RequestParam("state") String state) {
         // 인가 코드와 상태 값 처리
         saveAuthorizationCodeToDB(code, state);
         return ResponseEntity.ok("Authorization code received");
     }
     ```

4. **DB 저장**:
   - 수신한 인가 코드를 데이터베이스에 저장하여 이후 액세스 토큰 교환 시 사용함.

---

### 3. 데이터베이스 저장 방식

#### 필요성
- 인가 코드는 일회성 코드로, 짧은 시간 내에 사용되지 않으면 무효화됨.
- DB에 저장하여 코드를 추적하고 이후의 인증 요청에서 활용함.

#### 저장 구조 예시
- 테이블 설계:
  ```sql
  CREATE TABLE authorization_codes (
      id SERIAL PRIMARY KEY,
      code VARCHAR(255) NOT NULL,
      state VARCHAR(255) NOT NULL,
      created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  );
  ```

- 코드 저장 예시:
  ```java
  public void saveAuthorizationCodeToDB(String code, String state) {
      String query = "INSERT INTO authorization_codes (code, state) VALUES (?, ?)";
      jdbcTemplate.update(query, code, state);
  }
  ```

---

## 깨달은 것

1. **리디렉션과 컨트롤러의 역할**:
   - 리디렉션 URI를 통해 전달된 코드를 컨트롤러에서 처리하고, 이를 데이터베이스에 저장하여 안전하게 관리할 수 있음을 이해함.

2. **DB에 저장하는 이유**:
   - 코드의 추적과 액세스 토큰 발급을 위해 인가 코드를 안전하게 저장하고 관리하는 것이 중요함.

