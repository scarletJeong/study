# Maven과 Gradle 비교

## 소개

Maven과 Gradle은 자바 프로젝트의 빌드, 의존성 관리, 그리고 프로젝트 설정 자동화를 위한 도구로 널리 사용됨.   
두 도구의 차이점과 각 도구의 장단점을 정리함.

---

## 주요 학습 내용

### 1. Maven

#### 정의
- Maven은 Apache 소프트웨어 재단에서 개발한 빌드 자동화 및 프로젝트 관리 도구임.
- XML 파일(`pom.xml`)을 사용해 프로젝트 의존성, 빌드 설정, 플러그인을 정의함.

#### 특징
1. **XML 기반**:
   - 프로젝트 객체 모델(POM) 파일을 사용하여 빌드와 의존성을 정의함.

2. **관례에 의한 설정**:
   - 표준 디렉터리 구조와 관례를 기반으로 작업함.
   - 예: `src/main/java`와 같은 디렉터리 구조.

3. **생태계와 문서화**:
   - 널리 사용되며, 다양한 플러그인과 방대한 문서가 존재함.

#### 장단점
- **장점**:
  - 표준화된 구조로 인해 학습 곡선이 낮음.
  - 안정성과 성숙도가 높음.
  - 생태계와 문서가 잘 갖춰져 있음.

- **단점**:
  - XML 기반의 설정이 길어지고 가독성이 떨어질 수 있음.
  - 확장성과 유연성에서 Gradle에 비해 제한적임.

---

### 2. Gradle

#### 정의
- Gradle은 더 유연하고 강력한 빌드 도구로, 특히 Android 프로젝트에서 표준 도구로 사용됨.
- Groovy 또는 Kotlin DSL 파일(`build.gradle`)을 사용하여 의존성, 빌드 설정, 태스크를 정의함.

#### 특징
1. **DSL 기반**:
   - Groovy/Kotlin을 사용하여 선언적이고 유연한 설정 파일 작성 가능.

2. **Incremental Build**:
   - 빌드 과정의 변경 사항만 처리하여 빌드 속도를 크게 향상시킴.

3. **멀티 프로젝트 지원**:
   - 대규모 멀티모듈 프로젝트에 최적화된 기능 제공.

#### 장단점
- **장점**:
  - 유연한 설정과 빠른 빌드 속도를 제공함.
  - 멀티 프로젝트 및 대규모 프로젝트 관리에 적합함.
  - Groovy/Kotlin DSL로 가독성이 높고 확장성이 뛰어남.

- **단점**:
  - XML 기반 도구에 익숙한 사용자는 적응이 필요함.
  - 초기 학습 곡선이 상대적으로 높음.

---

### 3. Maven과 Gradle의 비교

| 특성                  | Maven                            | Gradle                          |
|-----------------------|----------------------------------|---------------------------------|
| 설정 방식             | XML(`pom.xml`)                  | Groovy/Kotlin DSL(`build.gradle`)|
| 빌드 속도             | 느림                             | 빠름                             |
| 유연성                | 제한적                           | 높음                             |
| 생태계 및 문서화       | 방대함                           | 상대적으로 적음                  |
| 멀티 프로젝트 지원      | 가능하나 제한적                   | 최적화되어 있음                   |
| 사용 사례             | 전통적인 Java 프로젝트           | Android 및 대규모 프로젝트       |

---

## 깨달은 것

1. **Maven과 Gradle의 역할 이해**:
   - Maven은 전통적인 Java 프로젝트에서 표준적인 도구로 사용되고, Gradle은 대규모 멀티모듈 및 Android 프로젝트에 적합함.
