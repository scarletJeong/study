- [x] IntelliJ에서 Spring Boot 프로젝트 확인 방법

# IntelliJ에서 Spring Boot 프로젝트 확인 방법

## 소개
IntelliJ IDEA에서 프로젝트가 Spring Boot로 설정되었는지 확인하는 방법을 정리함.
이를 통해 Spring Boot 프로젝트의 구성 요소를 빠르게 점검하고 실행할 수 있음.

## 주요 확인 방법

### 1. `build.gradle` 파일 확인
- **Spring Boot Starter 확인**:
  - `build.gradle` 파일에 아래와 같은 의존성이 포함되어 있는지 확인.
  ```gradle
  dependencies {  
      implementation 'org.springframework.boot:spring-boot-starter-data-jpa'  
      implementation 'org.springframework.boot:spring-boot-starter-web'  
      // 기타 의존성
  }
  ```
- **설명**:
  - `spring-boot-starter` 의존성은 프로젝트가 Spring Boot 기반으로 설정되었음을 나타냄.

### 2. `@SpringBootApplication` 어노테이션 확인
- **Spring Boot 어노테이션 확인**:
  - 메인 클래스에 `@SpringBootApplication` 어노테이션이 존재해야 함.

#### 예시 코드
```java
package com.repure.purewithme;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.security.servlet.SecurityAutoConfiguration;

@SpringBootApplication(exclude = { SecurityAutoConfiguration.class })
public class PurewithmeApplication {

    public static void main(String[] args) {
        SpringApplication.run(PurewithmeApplication.class, args);
    }
}
```

- **설명**:
  - `@SpringBootApplication`: Spring Boot 애플리케이션의 진입점을 표시.
  - `main` 메서드: 애플리케이션 실행을 시작.
  - `SecurityAutoConfiguration.class` 제외 설정: 특정 설정을 비활성화하는 예.

### 3. 내장 서버 실행 확인
- Spring Boot는 내장 Tomcat 서버를 포함하여 실행 가능한 서버를 자동으로 시작함.
- IntelliJ에서 애플리케이션 실행 시 콘솔에 다음과 같은 로그 메시지가 표시됨:

#### 실행 로그 예시
```
2024-11-20T09:26:44.849+09:00  INFO 15836 --- [  restartedMain] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
Tomcat started on port(s): 8080 (http)
```

- **설명**:
  - `Tomcat initialized with port 8080`: 내장 서버가 정상적으로 시작되었음을 나타냄.

---

## 깨달은 것

1. **Spring Boot 프로젝트 확인**:
   - `build.gradle` 파일과 `@SpringBootApplication` 어노테이션을 통해 쉽게 확인 가능.
2. **Spring Boot의 편리성**:
   - 내장 서버(Tomcat) 실행으로 별도 설정 없이 바로 애플리케이션 실행 가능.
3. **IntelliJ의 편리한 통합 환경**:
   - IDE 내에서 모든 설정과 실행 과정을 확인할 수 있어 생산성 향상.

[2024-11-20]
