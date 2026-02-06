
# Node.js 백엔드 프로젝트 초기 세팅 정리

## 1. 소개

Node.js 기반 백엔드 프로젝트를 시작하면서 초기 세팅 과정을 정리해둘 필요성을 느꼈다.

Express 서버 구성, 필수 패키지 설치, 폴더 구조 설계, 환경 변수 설정까지를 하나의 흐름으로 정리해 
**다음 프로젝트에서도 바로 꺼내 쓸 수 있는 기준**을 만드는 것이 목적이다.

---

## 2. 프로젝트 초기화

### 2.1 프로젝트 초기화
기본 `package.json` 파일을 생성한다.
```bash
npm init -y
```

### 2.2 필수 패키지 설치

```bash
npm install express dotenv cors helmet morgan
```

* express: 웹 서버 프레임워크
* dotenv: 환경 변수 관리
* cors: CORS 설정
* helmet: 보안 관련 HTTP 헤더 설정
* morgan: 요청 로그 관리

### 2.3 개발 의존성 설치
개발 중 코드 변경 시 서버를 자동으로 재시작하기 위한 도구


```bash
npm install --save-dev nodemon
```

---

## 3. 추가 패키지 설치
인증, 보안, 문서화까지 초기 단계에서 바로 사용할 수 있도록 구성.

```bash
# MySQL 연결
npm install mysql2

# Swagger (API 문서화)
npm install swagger-ui-express swagger-jsdoc

# 비밀번호 해싱
npm install crypto-js

# JWT 인증
npm install jsonwebtoken
```


---

## 4. 기본 폴더 구조

### 4.1 폴더 생성

```bash
mkdir src
mkdir src/routes
mkdir src/controllers
mkdir src/models
mkdir src/repositories
mkdir src/services
mkdir src/middleware
mkdir src/config
mkdir src/utils
```

### 4.2 프로젝트 구조
Java(Spring) 구조를 참고하되,Node.js 특성에 맞게 가볍게 계층을 분리했다.

```text
bomiora-back/
├── src/
│   ├── config/
│   │   ├── database.js
│   │   └── cors.js
│   ├── models/
│   │   └── User.js
│   ├── repositories/
│   │   └── UserRepository.js
│   ├── services/
│   │   └── UserService.js
│   ├── controllers/
│   │   └── UserController.js
│   ├── routes/
│   │   └── authRoutes.js
│   ├── utils/
│   │   └── passwordUtil.js
│   └── middleware/
│       └── errorHandler.js
├── .env
├── .gitignore
├── package.json
```



---

## 5. 환경 변수 설정

`.env` 파일은 보안상 자동 생성되지 않으므로 직접 생성한다.
민감 정보는 반드시 Git에 커밋되지 않도록 관리한다.

```env
PORT=9000
NODE_ENV=development

DB_HOST=bomiora0.mycafe24.com
DB_PORT=3306
DB_NAME=bomiora0
DB_USER=bomiora0
DB_PASSWORD=BMdiet8972!!

JWT_SECRET=your-secret-key-here
IMAGE_BASE_URL=https://bomiora.kr
```


---

## 6. 기타 필수 파일

* `.gitignore`
* `server.js` (Express 서버 시작점)
* `src/config/database.js` (MySQL 연결 설정)
* `src/config/cors.js` (CORS 설정)
* `src/middleware/errorHandler.js` (공통 에러 처리)
* `package.json` (start, dev 스크립트 추가)

---

## 7. 실행 방법

```bash
# 개발 모드
npm run dev

# 프로덕션 모드
npm start
```

---

## 8. 깨달은 점

* Node.js 백엔드는 초기 세팅이 이후 개발 속도에 큰 영향을 준다.
* 강제되는 규칙이 적은 만큼, 구조 설계에 대한 책임은 개발자에게 있다.
* 최소한의 계층 분리만으로도 유지보수성과 확장성이 크게 달라진다.
* 이런 초기 세팅을 문서로 남기는 것 자체가 다음 프로젝트를 위한 자산이 된다.

---

## 작성일

* 2026-02-04

```
```
