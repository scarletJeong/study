# JavaScript vs TypeScript 정리

## 1. 소개

이전에 JavaScript와 TypeScript의 차이를 한 번 공부한 적이 있었지만, 개념 위주로 훑는 수준에 그쳤다.

이후 실무에서 JavaScript 기반 프로젝트를 진행하면서 타입 안정성과 에러 관리의 중요성을 직접 체감하게 되었고,  
이를 계기로 두 언어를 다시 한 번 제대로 정리해보고자 이 문서를 작성하게 되었다.

이 문서는 JavaScript와 TypeScript의 차이를 정리하고, 최근 업계에서 TypeScript가 선호되는 이유를 이해하는 것을 목표로 한다.

---

## 2. JavaScript와 TypeScript 비교

| 항목 | JavaScript | TypeScript |
| --- | --- | --- |
| 타입 체크 | 사용해봐야 알 수 있음 | unknown + 타입 가드로 안전하게 |
| 에러 발견 시점 | 런타임 | 컴파일타임 |
| 자동완성 | 약함 | 강함 |
| 리팩토링 | 위험함 | 안전함 |
| 문서화 | 주석 필요 | 코드 자체가 문서 |
| 학습 곡선 | 쉬움 | 상대적으로 어려움 |
| 외부 API 데이터 | 검증 코드 직접 작성 | Zod 등으로 스키마 정의 |

---

## 3. 외부 데이터를 받을 때의 상황

### 시나리오: 외부 API에서 데이터 받기

### JavaScript - 타입을 모를 때

```js
async function getRobotData() {
  const response = await fetch('https://robot-api.com/data');
  const data = await response.json();

  // data의 구조를 알 수 없음
  // temperature가 존재하는지, 어떤 타입인지 실행해봐야 알 수 있음
  console.log(data.temperature); // undefined일 수도 있음
}
````

### TypeScript - unknown과 타입 가드 사용

```ts
async function getRobotData() {
  const response = await fetch('https://robot-api.com/data');
  const data: unknown = await response.json();

  if (isRobotData(data)) {
    console.log(data.temperature); // 타입이 보장됨
  }
}

function isRobotData(data: unknown): data is RobotData {
  return (
    typeof data === 'object' &&
    data !== null &&
    'temperature' in data &&
    typeof (data as any).temperature === 'number'
  );
}

interface RobotData {
  temperature: number;
  sensor: string;
  timestamp: Date;
}
```

---

## 4. TypeScript를 선호하는 이유

### 1) unknown 타입으로 안전하게 시작

```ts
const robotData: unknown = await fetchRobotData();

if (typeof robotData === 'object' && robotData !== null) {
  // 타입 확인 후 안전하게 사용
}
```

외부에서 들어오는 데이터는 신뢰하지 않고
항상 `unknown`에서 시작하는 것이 안전하다.

---

### 2) 컴파일타임에 에러 발견

```ts
const user = await userRepository.findByEmail(email);
user.password; // user가 null일 수 있다는 에러 발생
```

코드를 실행하기 전에 문제를 발견할 수 있어
실서비스 장애를 줄이는 데 큰 도움이 된다.

---

### 3) 자동완성과 개발 경험

```ts
const user: User = await userRepository.findByEmail(email);
user. // id, email, name, password 등 자동완성
```

타입 정보가 명확하면 IDE의 도움을 최대한 받을 수 있다.

---

### 4) 코드 자체가 문서가 됨

```ts
async findByEmail(email: string): Promise<User | null> {
  // email은 문자열
  // User 객체 또는 null 반환
}
```

함수 시그니처만 봐도 역할과 책임이 명확하다.

---

### 5) 리팩토링이 안전함

* 함수명, 필드명 변경 시 모든 사용처 추적 가능
* 누락된 부분은 컴파일 에러로 즉시 확인 가능

---

## + 런타임과 컴파일타임의 차이

| 구분         | 컴파일타임      | 런타임        |
| ---------- | ---------- | ---------- |
| 시점         | 코드 작성/빌드 시 | 코드 실행 시    |
| 에러 발견      | 실행 전       | 실행 중       |
| JavaScript | 거의 없음      | 대부분 여기서 발견 |
| TypeScript | 많은 에러 발견   | 일부만 발생     |
| 비유         | 시험 전 답안 검토 | 시험 도중 실수   |

### JavaScript 흐름

```
1. 코드 작성
   문법만 맞으면 통과

2. 서버 실행
   정상 시작

3. API 호출
   런타임 에러 발생
```

### TypeScript 흐름

```
1. 코드 작성
   타입 에러 발견

2. 코드 수정
   타입 문제 해결

3. 컴파일
   타입 체크 통과

4. 서버 실행
   안전하게 실행
```

---

## 6. 깨달은 점

- JavaScript는 빠르게 만들 수 있지만,규모가 커질수록 불안정성이 빠르게 드러난다.
- TypeScript는 초반에 신경 쓸 것이 많지만, 유지보수 단계에서 충분히 회수된다.
  특히 외부 API 연동, 협업, 리팩토링이 잦은 환경에서는TypeScript의 컴파일타임 안정성이 큰 장점이 된다.

---

2026-02-05





