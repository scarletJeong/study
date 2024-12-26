# PostgreSQL 학습 및 문제 해결 사례

## 소개

PostgreSQL 데이터베이스 작업 과정에서 경험한 주요 학습 내용과 문제 해결 사례를 정리한 거임. 이를 통해 데이터베이스 관리와 최적화 과정에서 겪은 실질적인 이슈와 그 해결 방안을 공유하려고 함.

## 주요 학습 내용

### 1. 데이터 이동과 관리

- **데이터 이동 도구**: PostgreSQL에서 `pg_dump`, `COPY`, DBeaver의 Export 기능을 활용해서 데이터를 이동함.
- **AWS RDS와 COPY 명령의 제약**:
  - AWS RDS에서는 보안상의 이유로 `COPY TO` 또는 `COPY FROM` 명령이 서버 파일 시스템에 접근할 수 없어서 문제가 생김.
  - 대신 클라이언트 측에서 `\copy` 명령을 사용하면 해결할 수 있음.

```sql
\copy public.users TO '/Downloads/users.csv' WITH CSV HEADER;
```

### 2. 테이블과 시퀀스 관리

#### (1) 테이블 생성 예제

아래는 `blood_sugar_alarm` 테이블 생성 SQL임:

```sql
CREATE TABLE public.blood_sugar_alarm (
    id int4 DEFAULT nextval('bloodsugaralarm_id_seq'::regclass) NOT NULL,
    email varchar NOT NULL,
    alarm_time timestamp NOT NULL,
    frequency varchar(50) NOT NULL,
    meal_type varchar(50) NOT NULL,
    enabled bool DEFAULT true NOT NULL,
    message text NULL,
    created_at timestamp DEFAULT CURRENT_TIMESTAMP NULL,
    updated_at timestamp DEFAULT CURRENT_TIMESTAMP NULL,
    CONSTRAINT bloodsugaralarm_pkey PRIMARY KEY (id)
);

-- Column comments
COMMENT ON COLUMN public.blood_sugar_alarm.email IS '이메일';
COMMENT ON COLUMN public.blood_sugar_alarm.alarm_time IS '알람 시간';
COMMENT ON COLUMN public.blood_sugar_alarm.frequency IS '알람 지속 기간';
COMMENT ON COLUMN public.blood_sugar_alarm.meal_type IS '식사 구분';
COMMENT ON COLUMN public.blood_sugar_alarm.enabled IS '알람 활성화 여부';
COMMENT ON COLUMN public.blood_sugar_alarm.message IS '메시지';
COMMENT ON COLUMN public.blood_sugar_alarm.created_at IS '알람 생성 시간';
COMMENT ON COLUMN public.blood_sugar_alarm.updated_at IS '알람 업데이트 시간';
```

#### (2) 시퀀스 문제 해결

- 문제: `ERROR: relation "blood_sugar_records_id_seq" does not exist`.
- 해결 방안:

**수동 시퀀스 생성**

```sql
CREATE SEQUENCE blood_sugar_records_id_seq
    START 1
    INCREMENT 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;
```

**시퀀스 자동 생성과 PRIMARY KEY 선언**

```sql
CREATE TABLE public.blood_sugar_records (
    id SERIAL PRIMARY KEY,
    sugar_level INT NOT NULL,
    measured_at TIMESTAMP NOT NULL
);
```

### 3. 에러 해결 사례

- **문제**: `ERROR: multiple primary keys for table "blood_pressure_record" are not allowed`.
- **원인**: 동일한 컬럼에 대해 중복된 PRIMARY KEY 제약 조건을 선언했기 때문임.
- **해결 방안**: 중복된 PRIMARY KEY 선언을 제거하면 됨.

#### 수정 전:

```sql
CREATE TABLE public.blood_pressure_record (
    id SERIAL PRIMARY KEY,
    email varchar(255) NULL,
    diastolic_level int4 NULL,
    CONSTRAINT blood_pressure_records2_pkey PRIMARY KEY (id)
);
```

#### 수정 후:

```sql
CREATE TABLE public.blood_pressure_record (
    id SERIAL PRIMARY KEY,
    email varchar(255) NULL,
    diastolic_level int4 NULL
);
```

## 깨달은 것

1. **관리형 PostgreSQL 환경 이해**: AWS RDS 같은 환경에서 발생할 수 있는 제약 조건을 이해하고 적합한 도구를 사용하는 게 중요함.
2. **SQL 테이블 정의와 시퀀스 관리**: 테이블과 시퀀스를 선언할 때 정확한 문법과 제약 조건을 설정해야 함.
3. **에러 메시지 분석 능력**: 에러 메시지를 통해 문제의 원인을 파악하고, 적절한 해결책을 적용하는 경험이 중요함.

[2024-12-20]



