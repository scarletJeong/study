
- [x] 크롤링 vs 배치
- [ ] jenkins 시작
- [x] 갑자기 오후 5시에 발생한.. 서버 문제

# 크롤링, 배치 처리 및 서버 문제 해결 사례

## 소개
크롤링과 배치 작업의 차이점, 그리고 서버 문제 해결 경험을 바탕으로 정리한 내용임. 
이를 통해 데이터 수집 및 처리 기술의 특성과 서버 운영에서 발생하는 문제를 다루는 방법을 공유함.

## 주요 학습 내용

### 1. 크롤링 vs 배치

#### 크롤링(Crawling)
- **정의**: 웹사이트나 특정 데이터 소스로부터 데이터를 자동으로 수집하는 프로세스임.
- **특징**:
  - 주기: 실시간 또는 특정 간격으로 동작함.
  - 입력: URL, API, 웹페이지 등임.
  - 출력: HTML, JSON, 텍스트 데이터 등 수집된 데이터.
  - 사용 기술: Scrapy, Puppeteer 등.

#### 배치(Batch)
- **정의**: 대규모 데이터를 일정 시간 간격으로 일괄 처리하는 작업임.
- **특징**:
  - 주기: 실시간보다는 일정 시간 간격으로 실행함.
  - 입력: CSV, 로그 파일, DB, 크롤링 데이터 등.
  - 출력: 정제된 데이터, 분석 결과, 보고서 등.
  - 사용 기술: ETL 도구(Talend), Hadoop, Apache Airflow 등.

| 작업 내용                        | 크롤링         | 배치 처리       |
| ---------------------------- | ----------- | ----------- |
| 데이터를 웹에서 가져오는 작업            | ✅ (주된 역할)   | ❌ (포함되지 않음) |
| 데이터를 가공, 분석, 변환하는 작업       | ❌ (포함되지 않음) | ✅ (주된 역할)   |
| 작업을 주기적으로 실행                | 크롤링 스케줄 가능 | ✅ 배치의 주요 특성 |

### 2. 서버 문제 해결 사례

#### 문제 상황
- Let's Encrypt로 SSL/TLS 인증서를 갱신하려다 도메인 소유권 검증 과정에서 실패함.
- **오류 메시지**:
  ```
  Domain: purewithme.com
  Type: unauthorized
  Detail: Invalid response from .../.well-known/acme-challenge/...: 404
  ```

#### 문제 원인
1. Nginx 설정에서 `/.well-known/acme-challenge/` 경로를 처리하지 못함.
2. Nginx가 Certbot이 만든 파일을 가로채거나 리다이렉트함.
3. 방화벽 요청 차단.
4. DNS 설정 오류로 도메인이 올바른 서버 IP를 가리키지 않음.

#### 해결 과정
1. `/etc/nginx/sites-available/default` 파일에서 8080 포트를 제거함.
2. Nginx를 재시작하고 Certbot으로 SSL 인증서를 갱신함.
3. 80포트와 443포트를 정상적으로 작동시켜 도메인 연결을 복구함.

#### 배운 점
- Nginx와 Spring Boot는 별개의 애플리케이션임.
- SSL 갱신 과정에서 Nginx가 멈추면 도메인 연결도 차단됨.
- 서버가 다운되었을 때 `http://IP:8080`으로 접근하면 Spring Boot 애플리케이션은 여전히 작동 중일 수 있음.

## 깨달은 것
1. **크롤링과 배치의 차이점**: 데이터 수집과 데이터 처리의 본질적인 차이를 이해함.
2. **서버 문제 해결의 중요성**: SSL 인증서 갱신 실패 같은 문제는 서버 전체에 영향을 줄 수 있음.
3. **Nginx와 Spring Boot의 독립성**: 각 요소가 어떻게 상호작용하는지 이해하는 것이 중요함.

[2024-12-16]


