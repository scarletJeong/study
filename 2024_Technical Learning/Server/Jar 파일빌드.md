# Jar와 War 빌드 파일: 개념과 생성 방법

## 개요

소프트웨어 개발에서 빌드 파일은 배포 및 실행을 위한 핵심 요소로, Jar와 War 두 가지 유형이 주로 사용됨.     
Spring Boot 프로젝트에서 빌드 파일을 생성하는 방법을 정리함.

---

## 빌드 파일의 종류

### 1. Jar (Java Archive)
- **Spring Boot**를 사용하는 경우 주로 사용.
- Spring Boot에는 **내장 서버**가 포함되어 있어 별도의 WAS(Web Application Server)가 필요하지 않음.
- 기본적으로 Tomcat 서버가 내장되어 있음.

### 2. War (Web Application Archive)
- **JSP**만으로 이루어진 애플리케이션에서 사용.
- **외부 WAS**(예: Tomcat, JBoss)를 사용하는 경우 적합.

---

## 빌드 및 실행 방법

### 방법 1: CMD(터미널)에서 실행

1. **배포 서버 내부에서 작업**:
   - 리눅스와 macOS: `ls`
   - Windows: `dir`

2. **프로젝트 내부 진입**:
   ```bash
   cd [Directory]
   ```

3. **빌드 실행**:
   ```bash
   ./gradlew build
   ```

4. **빌드된 Jar 파일 위치로 이동**:
   ```bash
   cd ./build/libs
   ```

5. **Jar 파일 실행**:
   ```bash
   java -jar [file-name]
   ```

   - 빌드된 파일 이름은 `build.gradle` 파일 내부의 버전 설정을 기준으로 생성됨.
   - 예: `프로젝트명-0.0.1-SNAPSHOT.jar`

6. **프로세스 강제 종료 (필요 시)**:
   ```bash
   sudo lsof -i :[port-number]
   kill [PID]
   ```

### 방법 2: IntelliJ IDEA를 사용한 Jar 파일 생성

1. **프로젝트 열기**:
   - IntelliJ에서 프로젝트를 열고 빌드 설정을 확인.
2. **빌드 구성**:
   - Gradle 또는 Maven을 사용하여 빌드 파일 생성.
3. **Jar 파일 생성**:
   - `build/libs` 디렉토리에서 Jar 파일 확인.

---

## 깨달은 점

1. **Jar와 War의 사용 차이**:
   - 내장 서버가 포함된 Spring Boot는 Jar를 기본으로 사용하며, 외부 WAS가 필요한 경우 War를 사용함.

2. **빌드 및 배포 프로세스 이해**:
   - CMD와 IntelliJ IDEA를 활용해 빌드 파일을 생성하고 실행하는 과정 학습.




참고 : 
https://isaac-christian.tistory.com/entry/AWS-Spring-Boot-%EC%9B%B9-%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98%EC%9D%84-AWS%EC%99%80-Mobaxterm%EC%9C%BC%EB%A1%9C-%EC%84%9C%EB%B2%84%EC%97%90-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0
