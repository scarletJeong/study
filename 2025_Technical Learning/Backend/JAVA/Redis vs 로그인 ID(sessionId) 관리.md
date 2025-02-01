# Redis vs 로그인 ID(sessionId) 관리

## 소개
로그인 세션 ID(sessionId)는 일정 시간(예: 10분) 동안만 유효하며, 이후에는 만료되어 더 이상 사용할 수 없음.   
따라서 **데이터베이스에 영구 저장할 필요 없이 메모리 내에서 관리**하는 것이 효과적임.
이를 위해 **Redis**와 같은 **외부 캐시 시스템**을 사용할 수 있으며, 또는 **서버 메모리**를 활용하여 직접 만료 시간을 관리할 수도 있음.

## 주요 학습 내용

### 1. 외부 캐시 시스템이란?

#### Redis란?
- Redis = Remote Dictionary Server
- **메모리 데이터 저장소**로, 데이터를 빠르게 읽고 쓸 수 있도록 지원함. 
- 주로 캐싱, 세션 관리, 메시지 브로커 등의 용도로 사용됨.

#### Redis의 주요 특징
- **데이터를 메모리에 저장하여 초고속 성능 제공**
- **Key-Value 기반의 NoSQL 데이터 저장소**
- **만료 시간 설정 가능 (TTL: Time-To-Live 지원)**
- **데이터 영속성 옵션 제공 (RDB, AOF 모드 지원)**
- **분산 환경에서 활용 가능**

<br>

### 2.로그인 세션 ID 관리 방법
#### 1) Redis를 활용한 로그인 세션 ID 관리

#### 1-1) 구현 
```java
import redis.clients.jedis.Jedis;
import java.util.UUID;

public class QuoteService {
    private Jedis jedis;

    public QuoteService() {
        this.jedis = new Jedis("localhost"); // Redis 연결
    }

    // 송금 견적서 생성
    public String createQuote(QuoteRequest request) {
        String quoteId = UUID.randomUUID().toString(); // UUID 생성
        long expireTimeInSeconds = 10 * 60; // 10분

        // 견적서 정보 저장 (10분 후 자동 삭제)
        jedis.setex(quoteId, expireTimeInSeconds, request.toJson());
        return quoteId;
    }

    // 로그인 세션 검증
    public Session getSession(String sessionId) {
        String sessionJson = jedis.get(sessionId);
        if (sessionJson == null) {
            throw new IllegalArgumentException("SESSION_EXPIRED");
        }
        return Session.fromJson(sessionJson);
    }
}
```
#### **Redis를 사용한 장점**
- **빠른 속도**: 모든 데이터를 메모리에 저장하여 접근 속도가 빠름
- **TTL(Time-To-Live) 설정 가능**: `setex` 명령어를 사용하여 자동 만료 기능 제공
- **서버 장애 복구 용이**: 데이터를 디스크에 저장하는 옵션 제공(RDB, AOF)
- **분산 시스템에서 유용**: 여러 서버에서 동일한 데이터를 공유 가능

---

#### 2) 서버 메모리에서 직접 관리하는 방법
Redis와 같은 외부 캐시 시스템을 사용하지 않고, 서버 메모리에서 직접 `sessionId`를 관리할 수도 있음.  
이 방법을 구현하려면 주로 `ScheduledExecutorService`와 같은 **스케줄링** 기능을 사용하여 **만료 시간을 체크**하고 **만료된 데이터는 자동으로 삭제**하도록 함.

#### 2-1) 방법

1. **`sessionId`와 만료 시간을 관리하는 객체를 만들고, 이를 메모리 내에서 저장**합니다.
2. `ScheduledExecutorService`를 사용하여 주기적으로 만료된 `sessionId`를 체크하고 삭제하는 작업을 수행합니다.
3. **만료된 `sessionId`는 더 이상 유효하지 않으며, 로그인 시 이를 확인하고 처리합니다.**

#### 2-2) 구현
```java
import java.util.concurrent.*;
import java.util.*;

public class SessionService {
    private static final long SESSION_EXPIRATION_TIME = 10 * 60 * 1000; // 10분
    private static final ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(1);
    private static final Map<String, Long> sessionExpirationMap = new HashMap<>();

    public SessionService() {
        scheduler.scheduleAtFixedRate(this::cleanUpExpiredSessions, 0, 1, TimeUnit.MINUTES); // 1분마다 체크
    }

    public String createSession(LoginRequest request) {
        String sessionId = UUID.randomUUID().toString();
        long expirationTime = System.currentTimeMillis() + SESSION_EXPIRATION_TIME;
        sessionExpirationMap.put(sessionId, expirationTime);
        return sessionId;
    }

    public Session getSession(String sessionId) {
        Long expirationTime = sessionExpirationMap.get(sessionId);
        if (expirationTime == null || System.currentTimeMillis() > expirationTime) {
            throw new IllegalArgumentException("SESSION_EXPIRED");
        }
        return new Session(sessionId);
    }

    private void cleanUpExpiredSessions() {
        long currentTime = System.currentTimeMillis();
        sessionExpirationMap.entrySet().removeIf(entry -> entry.getValue() <= currentTime);
    }
}

```

#### **서버 메모리 관리 방식의 특징**
- **외부 시스템 불필요**: Redis 없이 서버 내부에서 관리 가능
- **단순한 구조**: HashMap과 스케줄링 작업만으로 관리 가능
- **서버 재시작 시 데이터 손실**: 서버가 재시작되면 모든 `sessionId` 정보가 사라짐
- **확장성 부족**: 다중 서버 환경에서 공유가 어렵고, 분산 캐싱이 어려움


<br>

### 3. 비교 분석

| 방식 | 사용 기술 | 데이터 저장 위치 | 만료 처리 방식 | 확장성 |
|------|----------|----------------|--------------|------|
| **Redis** | Redis | 외부 캐시 시스템 | TTL(Time-To-Live) 지원 | 뛰어남 (클러스터링 가능) |
| **서버 메모리** | HashMap, ScheduledExecutorService | 서버 내부 메모리 | 스케줄러로 직접 삭제 | 낮음 (서버 재시작 시 데이터 손실) |

<br>

---

## 깨달은 것
1. Redis 사용의 안정성 및 확장성:
   - Redis는 대규모 시스템에서 더 효율적이고 안정적임.   -
   - TTL 기능으로 자동 만료가 가능해 관리가 쉬움.

2. 서버 메모리에서 sessionId 관리:
   - 작은 프로젝트에서는 서버 메모리로 sessionId를 관리할 수 있지만, 서버 재시작 시 데이터 손실이 발생할 수 있음.

3. 분산 시스템에서 Redis 사용:
   - Redis는 분산 시스템에서 서버 간 데이터 공유에 유용하며, 확장성이 뛰어남.
