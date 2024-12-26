- [x] AJAX와 @ResponseBody: JSON 처리 및 GET vs POST 비교

# AJAX와 @ResponseBody: JSON 처리 및 GET vs POST 비교

## 소개
AJAX 요청에서 `@ResponseBody`를 사용하여 데이터를 JSON 형식으로 처리하는 방법과 `GET` 및 `POST` 요청 간의 차이를 이해함. 이를 통해 클라이언트와 서버 간 데이터 전송을 최적화하고, 코드를 보다 일관되게 유지할 수 있음.

---

## 주요 학습 내용

### 1. AJAX와 @ResponseBody
- **`@ResponseBody`란?**
  - 컨트롤러 메서드에서 반환된 데이터를 HTTP 응답 본문에 직접 쓰는 방식.
  - 기본적으로 JSON 형식의 데이터 반환에 적합.

#### `@ResponseBody`를 사용한 JSON 반환
```java
@RequestMapping(value = "/getAuth.do", method = RequestMethod.POST)
public @ResponseBody Map<String, String> authorize(@RequestBody HashMap<String, Object> params) {
    String authUrl = "https://accounts.i-sens.com/auth/authorize"
                   + "?response_type=code"
                   + "&client_id=" + params.get("client_id")
                   + "&redirect_uri=" + params.get("redirect_uri");
    Map<String, String> responseMap = new HashMap<>();
    responseMap.put("redirectUrl", authUrl);
    return responseMap;
}
```

#### 문자열로 직접 반환
```java
@RequestMapping(value = "/getAuth.do", method = RequestMethod.POST)
public @ResponseBody String authorize(@RequestBody HashMap<String, Object> params) {
    String authUrl = "https://accounts.i-sens.com/auth/authorize"
                   + "?response_type=code"
                   + "&client_id=" + params.get("client_id")
                   + "&redirect_uri=" + params.get("redirect_uri");
    return "{\"redirectUrl\": \"" + authUrl + "\"}";
}
```

---

### 2. GET vs POST 요청
- **GET 요청**:
  - 데이터를 쿼리 문자열로 전송.
  - 캐싱 가능.
  - 전송 데이터 크기 제한 존재.

- **POST 요청**:
  - 데이터를 HTTP 요청 본문에 JSON 형식으로 전송.
  - 민감한 데이터 전송에 적합.
  - 데이터 크기 제한 없음.

#### AJAX 요청 구현
```javascript
$.ajax({
    url: '/getAuth.do',
    type: 'POST',
    data: JSON.stringify({ client_id: 'abc', redirect_uri: 'https://example.com' }),
    contentType: 'application/json',
    success: function(response) {
        console.log(response);
    }
});
```

---

### 3. ResponseEntity와 Map
#### `ResponseEntity` 사용
- HTTP 상태 코드와 응답 데이터를 함께 제어.
- 예:
  ```java
  @RequestMapping(value = "/getToken.do", method = RequestMethod.GET)
  public ResponseEntity<String> getToken(@RequestParam("userId") String userId) {
      String accessToken = tokenService.getAccessToken(userId);
      return accessToken != null
          ? ResponseEntity.ok(accessToken)
          : ResponseEntity.status(HttpStatus.NOT_FOUND).body("Token not found");
  }
  ```

#### `Map` 사용
- 간단한 JSON 응답 처리.
- 예:
  ```java
  @RequestMapping(value = "/getToken.do", method = RequestMethod.GET)
  public Map<String, String> getToken(@RequestParam("userId") String userId) {
      Map<String, String> response = new HashMap<>();
      response.put("accessToken", tokenService.getAccessToken(userId));
      return response;
  }
  ```

---

## 깨달은 것
1. **JSON 응답의 표준화**:
   - 클라이언트-서버 간 데이터 교환에 JSON 형식을 사용하면 표준화된 데이터 처리 가능.
2. **ResponseEntity의 유용성**:
   - 상태 코드와 응답 데이터를 세밀히 제어할 수 있어 유연한 API 설계 가능.
3. **GET vs POST 선택**:
   - 요청의 목적에 따라 적합한 HTTP 메서드를 선택하여 데이터 전송을 최적화해야 함.

AJAX와 Spring MVC의 기능을 활용하면 효율적이고 유지보수 가능한 데이터 전송 구조를 구현할 수 있음.

[2024-08-07]
