# Jenkins 파이프라인 스크립트

## 소개

Jenkins 파이프라인은 CI/CD 프로세스를 정의하고 자동화하는 강력한 도구임.   
Declarative Pipeline 스크립트의 기본 구조와 각 섹션의 역할, 주요 명령어 및 블록의 사용법에 대해 공부함.

---

## 주요 학습 내용

### 1. Jenkins 파이프라인 스크립트 기본 구조

```groovy
pipeline {
    agent any

    stages {
        stage('단계 1') {
            steps {
                echo '여기서 실행할 명령어를 작성합니다.'
            }
        }
        stage('단계 2') {
            steps {
                sh 'bash 명령어 실행 예: npm install'
            }
        }
        stage('단계 3') {
            steps {
                sh '빌드 또는 테스트 실행: npm run build'
            }
        }
        stage('Example') {
            steps {
                echo 'Hello World'
            }
        }
    }
    post {
        always {
            echo 'I will always say Hello again!'
        }
    }
}
```

---

### 2. 파이프라인 구성 요소

#### 1. `pipeline { }`
- 모든 Declarative Pipeline의 최상위 블록.
- 파이프라인의 구조와 작업 단계를 정의하며, 반드시 존재해야 함.
- 세미콜론 없이 작성.

#### 2. `agent`
- Jenkins 파이프라인을 실행할 노드(머신)를 정의.

| **설정**        | **설명**                            |
|---------------|-----------------------------------|
| `any`         | 모든 Jenkins 에이전트에서 실행.            |
| `none`        | 에이전트를 사용하지 않음.                  |
| `label '...'` | 특정 레이블이 있는 에이전트에서 실행.       |
| `docker`      | Docker 컨테이너에서 실행.               |
| `node`        | 특정 노드에서 실행.                     |

#### 3. `post`
- 특정 스테이지 이전 혹은 이후에 실행될 조건 블록을 정의.

| **설정**      | **설명**                                 |
|--------------|--------------------------------------|
| `always`    | 파이프라인 실행이 끝난 후 항상 실행.             |
| `changed`   | 이전 실행과 상태가 다를 경우 실행.               |
| `failure`   | 파이프라인이 실패했을 경우 실행.               |
| `success`   | 파이프라인이 성공했을 경우 실행.               |
| `unstable`  | 테스트 실패, 코드 위반 등으로 파이프라인이 불안정할 경우 실행. |
| `aborted`   | 강제로 중단된 경우 실행.                    |

#### 4. `stages`
- 파이프라인의 주요 작업 단계를 정의.
- 각 단계는 독립적으로 실행되며, 논리적 작업 흐름을 구성.

#### 5. `steps`
- 각 단계에서 실행할 명령어나 스크립트를 정의.

**주요 명령어**:
1. `sh`: 리눅스 명령어 실행.
2. `bat`: 윈도우 명령어 실행.
3. `echo`: 메시지 출력.
4. `git`: 소스코드 체크아웃.
5. `archiveArtifacts`: 빌드 결과물 저장.
6. `input`: 수동 승인 요구.

---

## 깨달은 것

1. **파이프라인 블록의 구조적 이해**:
   - `pipeline`, `agent`, `stages`, `steps`, `post` 등의 블록을 통해 CI/CD 파이프라인의 작업 흐름을 공부했음.
