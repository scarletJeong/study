
``` 
파이프라인 스크립트의 기본 구조

pipeline {
	/* Inesert Declaratice Pipeline */
	
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



1. pipeline { }
- 파이프라인의 top level은 반드시 pipeline{ } 블록으로 이루어져야 함.
- 세미콜론이 없음

2. sections
1) agent
> 	Jenkins 파이프라인을 실행할 노드(머신) 정의

| 설정          | 설명                    |
| ----------- | --------------------- |
| any         | 모든 Jenkins 에이전트에서 실행. |
| none        | 에이전트를 사용하지 않음.        |
| label '...' | 특정 레이블이 있는 에이전트에서 실행. |
| docker      | Docker 컨테이너에서 실행.     |
| node        | label 과 유사            |

2) post
>	특정 스테이지 이전 혹은 이후에 실행될 condition block

| 설정       | 설명                                  |
| -------- | ----------------------------------- |
| always   | 실행 끝나고나서 실행되는 step                  |
| changed  | previous run과 다른 status이면 실행되는 step |
| failure  | 실패하면 실행                             |
| success  | 성공하면 실행                             |
| unstable | test fail, code violation  등 일때 실행  |
| aborted  | 강제로 중지되었을 때 실행                      |

4. stages
> 	- 파이프라인의 단계(stage)정의
> 	- 각 단계는 독립적으로 실행

5. steps
>	- 각 단계에서 실행할 명령어나 스크립트
>	
>	- 명령어
>		- sh : 리눅스 명령어 실행
>		- bat : 윈도우 명령어 실행
>		- echo : 메시지 출력
>		- git :  소스코드 체크아웃
>		- archiveArtifacts : 빌드 아티팩트 저장
>		- input : 수동 승인 요구