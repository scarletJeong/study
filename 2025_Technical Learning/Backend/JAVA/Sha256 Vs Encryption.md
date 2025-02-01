# SHA-256 해싱과 암호화의 차이점

## 소개
비밀번호 처리를 구현하면서 SHA-256 해싱과 암호화(Encryption)의 차이가 궁금해져서 다룸.

## 주요 개념

### SHA-256 해싱 (Hashing)
- 입력값을 고정된 길이의 해시값으로 변환하는 일방향 암호화 방식.
- 원본 데이터를 복원할 수 없음.
- SHA-256은 해싱 알고리즘 중 하나로, 256비트 고정 길이의 해시값을 생성함.

- **특징**:
  - **일방향 암호화**: 원본 데이터 복구 불가.
  - **비밀번호 저장에 적합**: 안전한 비밀번호 저장 가능.
  - **무작위성**: 동일한 입력값에 대해 동일한 해시값 생성.
    
- **사용 사례**:
  - 비밀번호 저장: 사용자가 로그인할 때 입력한 비밀번호를 해싱하여 데이터베이스에 저장 후 비교. 로그인 시 입력한 비밀번호를 다시 해싱하여 저장된 해시값과 비교하는 방식
  - 데이터 무결성 확인: 파일이나 메시지가 변조되지 않았음을 검증하는 데 활용.
  
- **비밀번호 해싱 예제 (Java)**:
  ```java
  import java.security.MessageDigest;
  import java.security.NoSuchAlgorithmException;
  
  public class SHA256Hashing {
      public static String hashPassword(String password) throws NoSuchAlgorithmException {
          MessageDigest digest = MessageDigest.getInstance("SHA-256");
          byte[] hashedBytes = digest.digest(password.getBytes());
          StringBuilder hexString = new StringBuilder();
          for (byte b : hashedBytes) {
              hexString.append(String.format("%02x", b));
          }
          return hexString.toString();
      }
      public static void main(String[] args) {
          try {
              String password = "MySecurePassword";
              String hashedPassword = hashPassword(password);
              System.out.println("Hashed Password: " + hashedPassword);
          } catch (NoSuchAlgorithmException e) {
              e.printStackTrace();
          }
      }
  }
  ```
<br>

### 암호화 (Encryption)
- 데이터를 변환하여 보호하는 방법.
- 원본 데이터를 복호화하여 다시 얻을 수 있음.

- **특징**:
  - **양방향 암호화**: 복호화가 가능함.
  - **복호화 가능**: 특정 키를 사용하여 원본 데이터 복원 가능.
  - **대칭/비대칭 암호화**: 2가지 방식_대칭 암호화(하나의 키로 암호화하고 복호화)와 비대칭 암호화(서로 다른 키로 암호화하고 복호화) 방식 존재.

- **사용 사례**:
  - 데이터 전송: HTTPS(웹 서버와 클라이언트 간에 민감한 데이터를 안전하게 전송하기 위해 암호화된 연결) 등 보안 통신.
  - 파일 암호화: 중요 데이터 보호.
  - 디지털 서명: 메시지 출처와 무결성 증명.

- **암호화 예제 (Java AES)**:
  ```java
  import javax.crypto.Cipher;
  import javax.crypto.KeyGenerator;
  import javax.crypto.SecretKey;
  import javax.crypto.spec.SecretKeySpec;
  import java.util.Base64;
  
  public class EncryptionExample {
      public static String encrypt(String data, String key) throws Exception {
          Cipher cipher = Cipher.getInstance("AES");
          SecretKeySpec secretKey = new SecretKeySpec(key.getBytes(), "AES");
          cipher.init(Cipher.ENCRYPT_MODE, secretKey);
          byte[] encryptedData = cipher.doFinal(data.getBytes());
          return Base64.getEncoder().encodeToString(encryptedData);
      }
      public static String decrypt(String encryptedData, String key) throws Exception {
          Cipher cipher = Cipher.getInstance("AES");
          SecretKeySpec secretKey = new SecretKeySpec(key.getBytes(), "AES");
          cipher.init(Cipher.DECRYPT_MODE, secretKey);
          byte[] decodedData = Base64.getDecoder().decode(encryptedData);
          byte[] originalData = cipher.doFinal(decodedData);
          return new String(originalData);
      }
      public static void main(String[] args) {
          try {
              String data = "SensitiveData";
              String key = "1234567890123456";  // 16-byte key for AES-128
              String encrypted = encrypt(data, key);
              String decrypted = decrypt(encrypted, key);
              System.out.println("Encrypted Data: " + encrypted);
              System.out.println("Decrypted Data: " + decrypted);
          } catch (Exception e) {
              e.printStackTrace();
          }
      }
  }
  ```
<br>

### 해싱과 암호화 비교
| 특징 | 해싱 (Hashing) | 암호화 (Encryption) |
|------|--------------|----------------|
| 목적 | 데이터 무결성 보장, 원본 복원 불가 | 데이터 보호, 복호화 가능 |
| 복호화 | 불가능 | 가능 |
| 용도 | 비밀번호 저장, 데이터 무결성 검증 | 데이터 전송, 파일 보호, 개인정보 보호 |
| 기술 | SHA-256, MD5, bcrypt, Argon2 | AES, RSA, DES, 3DES |
| 비교 예시 | 	password → `5e884898da28047151d0e56f8dc6292773603d0d (해시값)`	| password → `encryptedData(암호화된 데이터)`| 

<br>

---

## 깨달은 점
1. **비밀번호 저장 방식**:
   - 해싱이 비밀번호 저장에 적합하며, 암호화는 데이터 보호 목적에 맞음.
2. **일방향과 양방향 차이**:
   - 해싱은 원본 복원이 불가능하지만, 암호화는 복호화가 가능함.

[2025.01.20]
