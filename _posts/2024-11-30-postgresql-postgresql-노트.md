---
layout: post
date: 2024-11-30 22:24:48 +0900
title: '[PostgreSQL] PostgreSQL 노트'
categories:
  - categorize-me
tags:
  - draft-permanantly
  - tag-me
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [PostgreSQL: Documentation](https://www.postgresql.org/docs/)

#### 테스트 환경 정보

- PostgreSQL 15.6 on aarch64-unknown-linux-gnu, compiled by gcc (GCC) 13.2.0, 64-bit


## 개요

PostgreSQL에 대해 알아낸 것들 기록.


## ⚠️ 식별자로 카멜케이스 사용 금지

PostgreSQL은 따옴표 없는 식별자를 자동으로 소문자로 변환한다. 그래서 camelCase로 정의한 객체를 큰따옴표 없이 찾지 못하는 문제가 있다:

```sql
select user_id, created_at from test_table_1; -- ✅

select "userId", "createdAt" from "testTable2"; -- ✅
select userId, createdAt from testTable2; -- ❌ Unable to resolve table 'testTable2' 
```

snake_case 쓰도록 하자...


## 개념 정리: 스키마, 사용자, 소유자, 역할, 객체와 권한

PostgreSQL의 권한 시스템은 대체로 Oracle과 유사하다.

테이블, 뷰, 시퀀스, 함수 같은 객체를 생성한 사용자(user)가 그 객체의 소유자(owner)가 된다. 소유자는 해당 객체의 읽기(SELECT), 쓰기(INSERT/UPDATE/DELETE), 구조 변경(ALTER), 삭제(DROP) 등 모든 권한을 자동으로 가지며, 다른 사용자나 역할(role)에게 권한을 부여할 수 있다. 객체의 소유자는 바꿀 수 있다.

역할(Role)은 권한의 묶음이다. 어떤 권한을 가진 역할을 정의하고, 특정 사용자에게 그 역할을 부여하는 방식으로 사용된다.

스키마(schema)는 테이블, 뷰, 시퀀스, 함수 등을 논리적으로 그룹화하는 일종의 네임스페이스(namespace)다. 개발자 입장에서는 패키지처럼 객체를 정리하고 이름 충돌을 방지하는 데 사용한다. 스키마도 객체이기 때문에 소유자가 존재하며 권한이 있어야 접근할 수 있다. 객체는 스키마간 이동이 가능하다.


## RLS, Row-Level Security

로우 단위로 접근 제한을 설정하는 PostgreSQL의 보안 기능. RLS가 활성화되면 기본적으로 superuser나 테이블 소유자 외엔 아무도 데이터에 접근할 수 없다.

RLS 활성화 후 테이블마다 정책(policy)을 작성하여, 특정 조건을 만족하는 사용자(혹은 익명 사용자)에게 데이터를 보여주거나 수정하도록 할 수 있다.

```sql
-- RLS 활성화
alter table my_table enable row level security;

-- 정책 강제 적용: superuser나 객체 소유자도 정책 적용
alter table my_table force row level security;
```

정책은 다음처럼 작성한다:

```sql
-- 로그인 사용자의 id와 일치하는 user_id의 로우만 select 가능
create policy "Enable select for users based on user_id"
on MY_TABLE
as PERMISSIVE
for SELECT
to public
using (
  user_id::text = current_setting('tx.current_user_id', true)
);

-- 로그인 사용자의 id와 일치하는 user_id만 insert 가능
create policy "Enable insert for users based on user_id"
on MY_TABLE
as PERMISSIVE
for INSERT
to public
with check (
  user_id::text = current_setting('tx.current_user_id', true)
);
```

`PERMISSIVE`는 기본 작동값이며 여러 정책 중 하나라도 통과하면 허용한다는 뜻이다. 이 외에 모든 정책을 통과해야만 허용하는 `RESTRICTIVE`가 있다.

SELECT와 DELETE는 `using`을, INSERT와 UPDATE 명령은 `with check`를 써줘야 적용된다. 만약 `ALL`을 쓰고 싶다면 아래처럼 둘 다 쓴다:

```sql
create policy "Users can select/modify/delete their own rows"
on MY_TABLE
as PERMISSIVE
for ALL
to public
using (
  user_id::text = current_setting('tx.current_user_id', true)
)
with check (
  user_id::text = current_setting('tx.current_user_id', true)
);
```

## current_setting()

위의 RLS 설명에서 예시로 작성한 정책들을 보면 `user_id`와 비교할 값을 `current_setting()`을 통해 가져오고 있는데, `current_setting()`은 GUC에 설정한 현재 값(current value)을 반환하는 함수다. 이 값은 `set`, `set local`, 또는 `postgresql.conf` 파일 등으로 설정된 GUC에서 가져온다.

```
current_setting ( setting_name [, missing_ok ] )
```

- `missing_ok`: 이 값이 `false`이고 설정 값을 찾지 못하면 에러가 발생하고 `true`일 땐 `null`을 반환한다. 기본값은 `false`.

```sql
-- 애플리케이션에서 트랜잭션 값 설정
set local tx.current_user_id = 'fixalot';

-- 조회
select current_setting('tx.current_user_id', true); -- 'fixalot'
```

애플리케이션이 PostgreSQL에 쿼리를 요청하기 전에, 커넥션에 설정된 트랜잭션 컨텍스트에 유저 식별값(예: 회원 UUID, 내부 ID 등)을 `SET LOCAL`로 저장해두고, PostgreSQL의 RLS 정책 또는 트리거, 함수 등이 그 값을 `current_setting()`으로 꺼내서 쓰는 방식이다.

ℹ️ 정책을 정의할 때는 바인딩 파라미터를 사용할 수 없기 때문에 이 방법을 쓴다.


## GUC, Global Unified Configuration

PostgreSQL은 내부적으로 설정값을 GUC이라 부르는데, 이 GUC는 아래처럼 유효 범위(scope)가 나뉜다:

|    GUC 범위    |                   예시                  |                        설명                       |
|:--------------:|:---------------------------------------:|:-------------------------------------------------:|
| postmaster     | `shared_buffers`                        | 서버 재시작 필요                                  |
| superuser      | `log_statement`                         | 슈퍼유저만 설정 가능                              |
| user / session | `TimeZone`, `search_path`,   `work_mem` | 세션 범위로 유효한 값. `SET`으로 지정함           |
| transaction    | `SET LOCAL app.user_id = 'abc'`         | 트랜잭션 범위에서만 유효한 값. `SET LOCAL`로 지정 |

`current_setting()`은 이 모든 GUC 중 현재 유효한 값을 반환할 수 있지만, 실제 활용에서 가장 자주 쓰이는 건 user 또는 transaction이다.

사용자 정의 GUC 값은 마침표`.`로 구분하는 접두어가 포함되어야 한다는 규칙이 있다:

```sql
set local app.user_id = 'abc';       -- ✅
set local custom.session = 'xyz';    -- ✅
set local myproject.tenant = 't001'; -- ✅
set local foo.bar = 'baz';           -- ✅

set local user_id = 'abc';           -- ❌ "unrecognized configuration parameter"
set local current_user = 'abc';      -- ❌ 예약어 충돌
set local foo = 'bar';               -- ❌ 점(.) 없음
```


## 데이터 타입

[https://www.postgresql.org/docs/current/datatype.html](https://www.postgresql.org/docs/current/datatype.html)

### text와 varchar의 타입

둘 다 가변 문자열을 저장하는 데이터 타입이다. 성능 차이는 거의 없고, 길이 제한을 하고 싶을 때 varchar를 쓴다고 한다.


## 메타 데이터

### 버전 확인

CLI 명령어:

```bash
postgres --version
```

쿼리:

```sql
select version();
```


## 시퀀스

시퀀스 객체의 이름이 `SEQ_OBJECT_NAME`일 때:

```sql
-- 시스템 카탈로그에서 시퀀스 전체 조회
select * from pg_catalog.pg_sequences;

-- 시퀀스 객체 조회
select * from SEQ_OBJECT_NAME;

-- 시퀀스 증가
select nextval('SEQ_OBJECT_NAME');

-- 특정 값으로 시퀀스 설정
select setval('SEQ_OBJECT_NAME', 10);

-- 현재 세션의 마지막 시퀀스 값 확인
select currval('SEQ_OBJECT_NAME');
```

`currval()`은 현재 세션에서 마지막으로 실행시킨 `nextval()`의 값을 반환한다. 다른 세션에서 증가시킨 값을 얻으려면 시퀀스 객체의 `last_value` 컬럼을 확인할 것.

```sql
select last_value from score_option_id_seq;
```


## EXPLAIN

실행계획을 보여주는 명령이다.

```
EXPLAIN [ ( option [, ...] ) ] statement
```

- `option`: `ANALYZE`, `VERBOSE`, `COSTS`, `SETTINGS`, `GENERIC_PLAN`, `BUFFERS`, `SERIALIZE`, `WAL`, `TIMING`, `SUMMARY`, `MEMORY`, `FORMAT` 중에 하나

```sql
explain analyze select * from examples where id = 123;
```

⚠️ `ANALYZE` 옵션은 실제 실행 시간과 기타 통계를 보여주는데, 이 옵션을 사용하면 작성한 쿼리가 진짜로 실행된다.


## 시간

### 현재 시간 조회하기

```sql
-- UTC 기준 시각 조회(timestamptz)
select now();

-- UTC 시각 지정
select (now() at time zone 'UTC');

-- 한국 시각 지정
select (now() at time zone 'KST');
select (now() at time zone 'Asia/Seoul');
```

## LIMIT

조회할 데이터의 수를 제한하는 기능. `OFFSET` 키워드로 시작 인덱스를 지정할 수 있다. MySQL, MariaDB의 `LIMIT`와 같다.

```sql
-- MY_COLUMN 기준 역순 정렬 후, 다섯 번째 로우 다음부터 2개 로우 조회
select *
from MY_TABLE
order by MY_COLUMN desc
limit 2 offset 5
```


## 데이터 타입 Data Types

[PostgreSQL: Documentation: 17: Chapter 8. Data Types](https://www.postgresql.org/docs/current/datatype.html)

### timestamptz, timestamp with time zone

특정 시간대를 기준으로 해석되어야 하는 시각 정보를 저장하는 데이터 타입. 시간값 뒤에 `+XX:XX`로 시간대를 표시하지만, 실제 저장흔 항상 UTC 기준이다.

세션에 따라 시간대가 자동으로 해석된다:

- 입력 시: 세션의 시간대 기준으로 해석한 뒤 → UTC로 변환해서 저장
- 조회 시: 저장된 UTC 값을 → 세션의 시간대 기준으로 자동 변환하여 표시

ℹ `now()` 함수가 반환하는 값의 데이터 타입이다.

⚠️ 이 타입의 기본값으로 `now() AT TIME ZONE 'KST'::text`를 설정하면 현재 시각보다 9시간 빠른 시각으로 저장될 수 있다. PostgreSQL에서 `now() AT TIME ZONE 'KST'`는 timestamp without time zone 타입으로 KST 시각을 반환하는데, 이걸 `timestamptz` 컬럼에 넣으면 그대로 UTC로 해석, 저장되기 때문이다.


## ETC.

### DBMS 툴과 연결할 때

데이터그립 같은 DBMS 툴을 사용하여 접속하려는 경우 Session pooler 항목을 찾아볼 것. (Direct connection이나 Transaction pooler는 사용하면 안 됨)
