# Spring과 JSON 연동, 로그 관리, 다국어 처리 이해

## 소개

Spring을 활용한 웹 애플리케이션에서 JSON 연동, 로그 관리, 다국어 처리 방식을 학습함.    
Spring MVC 설정 파일에서 JSON 처리를 위한 연동 방식, Log4j를 활용한 로그 관리, 그리고 메시지 파일을 통한 다국어 처리에 대해 자세히 정리함.

---

## 주요 학습 내용

### 1. AJAX와 JSON 연동

#### JSON 연동 필요성
- 클라이언트-서버 간 비동기 통신(AJAX)에서 JSON 데이터 형식으로 응답을 주고받기 위해 Spring과 JSON 연동이 필요함.

#### 설정 위치
- `WEB-INF > config > springmvc > dispatcher_servlet.xml` 파일에서 JSON 연동 설정을 관리함.

#### 설정 예시
```xml
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
    <property name="messageConverters">
        <list>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter" />
        </list>
    </property>
</bean>
```
- `MappingJackson2HttpMessageConverter`를 통해 객체를 JSON으로 변환하여 응답함.

#### 동작 과정
1. 클라이언트에서 AJAX 요청을 보냄.
2. Spring Controller에서 요청을 처리하고 데이터를 반환함.
3. `MappingJackson2HttpMessageConverter`를 사용해 데이터를 JSON 형식으로 변환하여 클라이언트에 전달함.

---

### 2. 로그 관리(Log4j 설정)

#### 로그 관리의 중요성
- `System.out.println()` 대신 로그를 사용해 애플리케이션의 상태와 실행 과정을 기록함.
- SQL 실행 로그와 같은 중요한 정보를 효율적으로 관리하기 위해 Log4j를 활용함.

#### 설정 위치
- `log4j.xml` 파일에서 로그 설정을 관리함.

#### 설정 예시
```xml
<appender name="console" class="org.apache.log4j.ConsoleAppender">
    <layout class="org.apache.log4j.PatternLayout">
        <param name="ConversionPattern" value="%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n" />
    </layout>
</appender>

<appender name="file" class="org.apache.log4j.RollingFileAppender">
    <param name="File" value="logs/app.log" />
    <param name="MaxFileSize" value="10MB" />
    <param name="MaxBackupIndex" value="5" />
    <layout class="org.apache.log4j.PatternLayout">
        <param name="ConversionPattern" value="%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n" />
    </layout>
</appender>

<root>
    <priority value="debug" />
    <appender-ref ref="console" />
    <appender-ref ref="file" />
</root>
```
- `console`과 `file` 두 가지 방식으로 로그를 출력함.
- SQL 로그를 포함한 실행 정보가 `logs/app.log` 파일에 저장됨.

#### 사용 예시
```java
import org.apache.log4j.Logger;

public class Example {
    private static final Logger logger = Logger.getLogger(Example.class);

    public void executeQuery() {
        try {
            // SQL 실행 로직
            logger.info("SQL 실행 시작");
        } catch (Exception e) {
            logger.error("SQL 실행 오류", e);
        }
    }
}
```

---

### 3. 다국어 처리

#### 다국어 처리의 필요성
- 웹 애플리케이션이 다양한 언어를 지원하기 위해 다국어 메시지 파일을 활용함.
- 메시지 파일에서 각 언어별 번역된 텍스트를 관리함.

#### 설정 위치
- Spring MVC에서 메시지 파일을 `dispatcher_servlet.xml`에서 정의함.

#### 설정 예시
```xml
<bean id="messageSource" class="org.springframework.context.support.ResourceBundleMessageSource">
    <property name="basename" value="messages/messages" />
    <property name="defaultEncoding" value="UTF-8" />
</bean>
```
- `messages/messages`는 다국어 처리를 위한 프로퍼티 파일 위치를 지정함.
- `UTF-8` 인코딩을 통해 다국어 지원을 강화함.

#### 메시지 파일 구조
- `messages_ko.properties`:
  ```properties
  greeting=안녕하세요
  ``

- `messages_en.properties`:
  ```properties
  greeting=Hello
  ```

#### 사용 예시
```java
@Autowired
private MessageSource messageSource;

public void displayMessage(Locale locale) {
    String message = messageSource.getMessage("greeting", null, locale);
    System.out.println(message);
}
```
- 로케일(Locale)을 기반으로 적절한 언어의 메시지를 출력함.

---

## 깨달은 것

1. **AJAX와 JSON 연동 방식 이해**:
   - Spring MVC와 JSON 연동을 설정하고, 클라이언트와 서버 간 데이터를 효과적으로 교환하는 방식을 알게 됨.

2. **효율적인 로그 관리 필요성**:
   - Log4j를 통해 SQL 실행 로깅 방식을 이해함.


