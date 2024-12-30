# MobaXterm을 활용한 JAR 파일 업로드 및 서버 관리

## 소개

MobaXterm을 사용하여 서버에 JAR 파일을 업로드하고, 서버를 관리 및 재시작하는 방법을 정리함.

---

## MobaXterm의 주요 기능

1. **SSH (Secure Shell)**
   - 원격 접속 프로토콜로, 네트워크 상에서 다른 컴퓨터에 로그인하거나 원격 작업을 수행 가능.
   - 사용 예: 클라우드 서버에 접속.

2. **FTP (File Transfer Protocol)**
   - 원격 파일 전송 프로토콜로, 서버 간 파일 전송을 지원.

3. **SFTP (Secure File Transfer Protocol)**
   - SSH를 기반으로 한 파일 전송 프로토콜로, 보안성을 강화함.

---

## 설치 및 기본 설정

### 1. MobaXterm 설치

- 공식 사이트에서 다운로드 후 설치.

### 2. 서버 접속 설정

1. **Session 생성**:
   - MobaXterm 실행 후 `Session → SSH` 선택.

2. **접속 정보 입력**:
   - **Remote host**: 원격 서버의 IP 주소 또는 도메인 (예: `i6a604.p.ssafy.io`)
   - **Username**: 서버 사용자 이름 (예: `ubuntu`)
   - **Advanced SSH settings**:
     - `Use private key`에서 PEM 키 등록.
     - 서버의 SSH 키를 `.pem` 형식으로 로컬에 다운로드한 후 등록.

3. **MySQL과 Workbench 연결**

	3-1. **MySQL 접속**:
   ```bash
   sudo /usr/bin/mysql -u root -p
   ```
	3-2. **외부 접근 가능 유저 생성**:
   ```sql
   CREATE USER 'root'@'%' IDENTIFIED BY '[password]';
   GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
   ```

---

## JAR 파일 업로드 및 서버 관리

### 1. 서버 작업 흐름

1. **서버 IP 확인**:
   ```bash
   ip addr show
   ```
   - 서버의 네트워크 인터페이스와 IP 주소 확인.

2. **서버 확인 및 종료**:
   - 실행 중인 Java 프로세스 확인:
     ```bash
     ps aux | grep java
     ```
     출력 예시:
     ```
     ubuntu 1234 0.2 1.2 3403448 40124 ?  Sl  10:00 0:10 java -jar myapp.jar
     ```
   - 특정 포트에서 실행 중인 프로세스 확인:
     ```bash
     sudo lsof -i :8080
     ```
   - 프로세스 종료:
     ```bash
     sudo kill -9 1234
     ```

3. **JAR 파일 업로드**:
   - MobaXterm의 SFTP를 활용해 JAR 파일을 서버의 지정 디렉토리로 전송.

4. **JAR 파일 실행**:
   ```bash
   nohup java -jar /home/username/myapp.jar &
   ```
   - 예: `nohup java -jar purewithme/purewithme-0.0.1-SNAPSHOT.jar &`

5. **실행 로그 확인**:
   ```bash
   tail -f nohup.out
   ```

---

## 깨달은 점

MobaXterm을 활용해 서버 관리 작업을 자동화하고, 효율적인 배포 환경을 구성할 수 있음을 학습함.





참고 : 
https://exuzii.tistory.com/entry/Spring-Boot-41-%EC%84%9C%EB%B2%84-%EB%B0%B0%ED%8F%AC-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0%EC%84%9C%EB%B2%84-%EC%84%A4%EC%A0%95-%EB%B0%B0%ED%8F%AC-%ED%8C%8C%EC%9D%BC-%EC%83%9D%EC%84%B1-%EB%B0%8F-%EC%A0%84%EC%86%A1
