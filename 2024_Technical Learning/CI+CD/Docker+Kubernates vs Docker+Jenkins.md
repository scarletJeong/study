# Docker+Kubernates vs Docker+Jenkins

## 소개
    
Docker, Kubernetes 그리고 Jenkins로  개발 및 배포 환경을 구축하는 방법을 정리함.

---

## 주요 학습 내용

### 1. Docker, Kubernetes, Jenkins의 역할

#### Docker
- 소프트웨어를 컨테이너로 패키징하고 실행할 수 있는 도구.

**특징**:
1. 컨테이너화:
   - 일관된 실행 환경 제공.
2. 이미지 관리:
   - Docker 이미지를 통해 애플리케이션 실행 가능.
3. 경량화:
   - 가상 머신보다 빠르고 리소스 소비가 적음.

**구성 요소**:
1. Dockerfile:
   - 컨테이너 이미지를 정의.
2. Docker Image:
   - 컨테이너 실행 템플릿.
3. Docker Container:
   - 실행 중인 애플리케이션 환경.

#### Kubernetes
- 컨테이너화된 애플리케이션을 배포, 관리, 확장하는 플랫폼.

**기능**:
1. 컨테이너 오케스트레이션:
   - 자동 배포, 확장, 복구.
2. 셀프 힐링:
   - 실패한 컨테이너를 자동 재시작.
3. 자동 확장:
   - 트래픽 증가 시 컨테이너 수를 자동으로 조정.

**구성 요소**:
1. Node:
   - 컨테이너 실행 단위.
2. Pod:
   - 컨테이너 그룹.
3. Deployment:
   - 컨테이너 관리 API.
4. Service:
   - 외부와 통신하는 네트워크 인터페이스.

#### Jenkins
- CI/CD 파이프라인을 자동화하는 도구.

**기능**:
1. CI/CD 파이프라인 구축:
   - 코드 푸시 시 자동 빌드, 테스트, 배포.
2. 플러그인 지원:
   - Docker, Kubernetes, GitHub 등과 통합 가능.
3. 자동화 작업 관리:
   - 스크립트 기반 복잡한 작업 자동화.

**파이프라인 구조**:
1. Declarative Pipeline:
   - YAML과 비슷한 형식으로 정의.
2. Scripted Pipeline:
   - Groovy 스크립트로 작업 정의.

---

### 2. Docker + Jenkins vs Docker + Kubernetes

#### Docker + Jenkins로 CI/CD 구축
1. Docker:
   - 애플리케이션을 컨테이너화하여 실행.
2. Jenkins:
   - CI/CD 파이프라인을 자동화.

**장점**:
- Jenkins를 통해 빌드, 테스트, 배포 과정을 자동화 가능.
- Docker와 통합하여 환경에 구애받지 않고 실행 가능.

**단점**:
- Jenkins는 컨테이너의 상태 관리나 확장 기능을 제공하지 않음.

#### Docker + Kubernetes
1. Kubernetes:
   - 컨테이너 상태 관리, 확장, 복구.

**장점**:
- 자동 복구 및 확장 지원.
- 트래픽 증가 시 자동으로 컨테이너 추가.
- 멀티 클라우드 지원.

**단점**:
- CI/CD 파이프라인은 제공하지 않으므로 추가 도구 필요.

**비교**:
| **기능**         | **Jenkins**       | **Kubernetes**    |
|------------------|------------------|------------------|
| CI/CD 자동화      | 가능             | 불가능             |
| 컨테이너 상태 관리 | 불가능            | 가능               |
| 확장성            | 수동 관리 필요     | 자동 확장 지원       |
| 러닝 커브          | 쉬움              | 어려움             |

---

### 3. Kubernetes를 더 선호하는 이유

1. **컨테이너 기반 애플리케이션의 실행 환경 관리**:
   - Kubernetes는 컨테이너의 상태를 지속적으로 모니터링하며 복구와 확장을 자동화함.

2. **대규모 운영 환경에 적합**:
   - 단일 애플리케이션 배포보다, 수백 개의 컨테이너와 서비스를 관리하는 데 효과적임.

3. **멀티 클라우드 및 온프레미스 지원**:
   - AWS, Azure와 같은 클라우드뿐만 아니라 온프레미스 환경에서도 클러스터를 구성 가능.

4. **자동화된 복구 및 확장**:
   - 장애 발생 시 복구 작업을 자동으로 처리하며, 트래픽 증가에 따라 확장 가능.

---

### 4. Jenkins 하나만 충분할까?

1. Jenkins는 CI/CD에 특화되어 있어 코드 변경 시 빌드, 테스트, 배포를 쉽게 관리 가능함.

2. Jenkins와 Docker 연동:
   - Jenkins가 GitHub에서 코드 변경 사항을 가져와 Docker 이미지를 빌드하고 컨테이너로 실행하여 기본 CI/CD 기능 구현 가능.

3. Jenkins의 한계:
   - 컨테이너 상태 관리, 확장, 복구 기능이 부족함.
   - Kubernetes 같은 컨테이너 오케스트레이션 도구와 결합해야 애플리케이션의 안정성과 확장성을 보장 가능.

---

### 5. Kubernetes 기반 배포 과정

#### 배포 수행 방법
1. **수동 배포**:
   - Docker 이미지 빌드.
   - 이미지를 컨테이너 레지스트리에 푸시.
   - Kubernetes Deployment 업데이트.
   - 배포 확인.

2. **자동 배포**:
   - Jenkins, GitLab CI/CD 같은 도구를 통해 파이프라인 구축.
   - ArgoCD, FluxCD 같은 GitOps 도구 활용.

---

## 깨달은 것

1. **CI/CD와 컨테이너화의 중요성**:
   - Docker와 Jenkins를 통해 자동화된 소프트웨어 배포의 효율성을 경험.

2. **Kubernetes의 강력한 컨테이너 관리 기능**:
   - 확장성과 복구 능력을 통해 대규모 애플리케이션 환경 관리 가능.