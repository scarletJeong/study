# GitHub Actions
## 개요

GitHub Actions는 깃허브에서 제공하는 CI/CD 플랫폼으로, 소프트웨어 개발에서 빌드, 테스트, 배포 작업을 자동화하는 **파이프라인**을 생성할 수 있음.

---

## 주요 개념 및 구성 요소

### 1. Workflow
- 한 개 이상의 Job을 실행할 수 있는 자동화된 작업.
- YAML 파일 형식으로 정의되며, 특정 이벤트에 의해 실행됨.

### 2. Event
- Workflow 실행을 발동시키는 특정한 활동.
- 예시: Push 이벤트, Pull Request 이벤트, Issue 이벤트 등.
  - 깃허브에 소스 코드를 푸시하면 발생하는 작업.

### 3. Jobs
- 한 가지 러너(Runner) 안에서 실행되는 여러 Step의 모음.
- 특징:
  - Step은 일종의 Shell Script처럼 실행됨.
  - Step 간에 데이터 공유 가능.
  - Jobs는 병렬 실행 가능하며, 다른 Job에 의존 관계를 설정할 수 있음.

### 4. Actions
- 복잡하고 자주 반복적인 작업을 정의한 커스텀 애플리케이션.
- 장점:
  - Workflow 파일에서 자주 반복되는 코드를 줄일 수 있음.
  - GitHub 마켓플레이스를 통해 공용 Action 또는 다른 사용자가 만든 Action을 활용 가능.

---

## GitHub Actions의 워크플로우 실행 구조

### 실행 흐름
1. 이벤트 발생:
   - Push, Pull Request 등 이벤트가 트리거로 작동.
2. Workflow 파일 실행:
   - `.github/workflows/` 경로에 정의된 YAML 파일 실행.
3. Job 실행:
   - 정의된 Job이 순차적으로 또는 병렬적으로 실행됨.
4. Step 실행:
   - 각 Job 내 Step이 순서대로 실행되며, 필요 시 데이터 공유 가능.

### 예시 YAML 파일
```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

      - name: Build project
        run: npm run build
```

---

## 깨달은 점

1. **Jobs와 Steps의 구성 이해**:
   - 병렬 처리와 의존 관계 설정을 통해 효율적인 CI/CD 파이프라인 구현 가능.

