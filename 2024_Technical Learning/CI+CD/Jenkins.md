
# Jenkins
= CI (continuous Integration) tool

## 소개

Jenkins는 CI/CD 도구로, 소프트웨어 개발에서 코드 빌드, 테스트, 배포를 자동화하여 생산성을 높이는 데 필수적인 역할을 함. 다양한 아이템(Item) 유형과 스크립트 기반의 파이프라인 설정을 통해 유연하고 효율적인 CI/CD 환경을 구축할 수 있음.

---

## 주요 학습 내용

### 1. Jenkins 아이템(Item)

#### Jenkins 아이템이란?
- 빌드 작업을 실행하기 위한 단위로, 프로젝트 또는 작업(Job)을 의미함.
- Jenkins를 통해 CI/CD 파이프라인의 중심 역할을 수행.

#### 아이템 종류 및 설명

| **아이템 종류**                | **설명**                                                          | **사용 사례**            |
|----------------------------|---------------------------------------------------------------|---------------------|
| **Freestyle project**      | 가장 기본적인 프로젝트 유형. 간단한 빌드, 테스트, 스크립트 실행 등에 사용.             | 간단한 빌드 작업        |
| **Pipeline**               | **Jenkinsfile**을 사용해 파이프라인 작업을 스크립트로 정의. 복잡한 CI/CD 작업에 유용. | CI/CD 파이프라인 자동화 |
| **Multi-branch Pipeline**  | 여러 Git 브랜치를 자동으로 감지해 각 브랜치에 맞는 파이프라인을 실행.               | 브랜치별 다른 작업 실행    |
| **Folder**                 | 여러 아이템을 하나의 폴더에 그룹화해 관리.                                    | 프로젝트 그룹화 및 관리   |
| **GitHub Organization**    | GitHub 리포지토리 전체를 스캔하여 파이프라인을 자동으로 실행.                     | GitHub 전체 빌드 관리    |
| **External Job**           | Jenkins 외부에서 실행되는 작업을 모니터링.                                    | 외부 스크립트 실행 관리   |
| **Multi-configuration**    | 여러 환경(OS, 브라우저, JDK 버전)에서 병렬로 테스트하거나 빌드를 실행.               | 크로스 플랫폼 테스트      |


.................                                                                                                                        				       
#### Freestyle vs Pipeline

| **특징**         | **Freestyle**                                              | **Pipeline**                                                     |
|----------------|-------------------------------------------------------|-------------------------------------------------------------|
| **설정 방식**    | GUI 기반 설정                                            | 코드 기반 설정 (Jenkinsfile)                                    |
| **변경 관리**    | Jenkins GUI에서 수동으로 변경                             | Git 버전 관리 가능                                              |
| **유연성**      | 단순 작업에 적합                                         | 복잡한 CI/CD 파이프라인 정의 가능                                  |
| **장점**        | 설정이 직관적이고 간단                                   | 파이프라인 흐름 제어, 상태 추적, 통계 제공                              |
| **단점**        | CI/CD 과정 시각화가 어려움                                | 문법 학습 필요, 스크립트 작성 번거로움                                 |

---

### 2. Jenkins 파이프라인 스크립트 작성

#### 스크립트 작성 관리 방법
1. Jenkins 웹 내에서 스크립트 작성하여 관리 → **Pipeline Script (Default)**
2. 프로젝트 내에서 `Jenkinsfile`로 스크립트를 작성하여 관리 → **Pipeline Script from SCM**

#### 스크립트 문법 종류
1. **Declarative Pipeline**
   - Groovy 기반이나 초보자도 쉽게 작성 가능.
   - 최상단에 `pipeline` 키워드로 시작.

2. **Scripted Pipeline**
   - Groovy 기반으로 복잡한 작업에 적합.
   - 최상단에 `node` 지시어로 시작.

**문법 비교**:
- Declarative는 코드가 간결하고 직관적.
- Scripted는 고급 기능 활용 가능.

---

## 깨달은 것

1. **아이템의 다양성과 활용**:
   - Freestyle, Pipeline, Multi-branch Pipeline 등 다양한 아이템을 프로젝트 성격에 맞게 선택할 수 있음을 이해함.

2. **파이프라인 설정의 중요성**:
   - Declarative와 Scripted 문법을 통해 복잡한 CI/CD 흐름을 유연하게 제어할 수 있음을 학습함.
   - 
---

참고 : https://blog.voidmainvoid.net/104#google_vignette
