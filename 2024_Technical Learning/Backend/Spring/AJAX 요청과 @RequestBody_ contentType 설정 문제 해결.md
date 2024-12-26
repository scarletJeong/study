- [x] AJAX 요청과 @RequestBody: contentType 설정 문제 해결

# AJAX 요청과 @RequestBody: contentType 설정 문제 해결

## 소개
AJAX 요청 시 Spring MVC 컨트롤러의 `@RequestBody`를 사용할 때 발생할 수 있는 `contentType` 미설정 문제와 이를 해결하는 방법을 정리함. 이를 통해 서버와 클라이언트 간의 데이터 전송 오류를 방지할 수 있음.

## 주요 학습 내용

### 1. 문제 상황
- **에러 메시지**:
  ```
  could not resolve view with name 'cmmn/egovError' in servlet with name 'action'
  ```
- **원인**:
  - AJAX 요청에서 `contentType`을 설정하지 않아 Spring 컨트롤러에서 데이터를 제대로 파싱하지 못함.
  - 서버가 요청 데이터를 어떻게 해석해야 할지 `contentType` 헤더를 통해 결정함.

---

### 2. 주요 개념

#### 1. @RequestBody
- **정의**:
  - HTTP 요청의 본문을 Java 객체로 변환하는 데 사용.
- **특징**:
  - 변환 과정에서 Spring MVC가 요청의 `contentType` 헤더를 참고함.

#### 2. contentType
- **정의**:
  - 서버로 데이터를 보낼 때 데이터의 타입을 지정하는 HTTP 헤더.
- **기본값**:
  - `application/x-www-form-urlencoded; charset=utf-8`.
- **자주 사용하는 값**:
  - `application/json`.

#### 3. 기본 동작
- AJAX 요청에서 `contentType`을 지정하지 않으면 기본적으로 다음과 같이 간주:
  - `application/x-www-form-urlencoded` 또는 `text/plain`.
  - 이 경우 `@RequestBody`는 데이터를 처리하지 못하고 에러 발생.

---

### 3. 해결 방법

#### 1. AJAX에서 contentType 설정
- **코드**:
  ```javascript
  $.ajax({
      url: '/example',
      type: 'POST',
      contentType: 'application/json',
      data: JSON.stringify({
          key: 'value'
      }),
      success: function(response) {
          console.log(response);
      }
  });
  ```
- **설명**:
  - `contentType`을 `application/json`으로 설정.
  - 데이터도 JSON 형식으로 전송.

#### 2. @RequestParam으로 변경
- **상황**:
  - 데이터가 URL 인코딩된 형식으로 전달되는 경우 `@RequestBody` 대신 `@RequestParam` 사용.
- **코드**:
  ```java
  @RestController
  public class ExampleController {
      @PostMapping("/example")
      public ResponseEntity<String> example(@RequestParam String param) {
          return ResponseEntity.ok("Received: " + param);
      }
  }
  ```

#### 3. 이중 JSON 갇힘 문제 방지
- **문제**:
  - AJAX에서 `data: JSON.stringify()`로 직렬화 후, `@RequestParam`으로 받으면 JSON이 중첩됨.
- **해결 방법**:
  - `@RequestBody`와 `contentType: application/json`을 올바르게 설정.

---

## 깨달은 것

1. **contentType 설정의 중요성**:
   - 서버가 데이터를 올바르게 파싱하려면 요청의 `contentType`을 반드시 설정해야 함.
2. **@RequestBody와 @RequestParam의 차이**:
   - 데이터 형식에 따라 적합한 어노테이션을 선택해야 함.
3. **AJAX 요청 디버깅**:
   - 브라우저의 네트워크 탭과 서버 로그를 활용하여 문제를 분석하고 해결 가능.

올바른 설정을 통해 클라이언트-서버 간 데이터 전송을 원활히 수행할 수 있음.


[2024-08-02]
