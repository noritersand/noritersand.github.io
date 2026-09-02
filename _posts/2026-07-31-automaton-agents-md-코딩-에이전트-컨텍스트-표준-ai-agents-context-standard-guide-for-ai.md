---
layout: post
date: 2026-07-31 16:06:18 +0900
title: '[Automaton] AGENTS.md: AI 에이전트 컨텍스트 표준'
categories:
  - automaton
tags:
  - agents-md
  - ai-agent
  - automaton
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [AGENTS.md](https://agents.md/)


## 개요

`AGENTS.md`는 코딩 에이전트용 컨텍스트 파일의 개방형 표준이다.

기존에는 `CLAUDE.md`, `.cursorrules` 같은 파일을 프로젝트 루트에 두고 빌드 방법, 코딩 컨벤션, 테스트 방법 같은 프로젝트 규칙을 적어두는데, 이 방식의 국제 표준이라고 보면 된다.

AI 코딩 에이전트는 세션마다 컨텍스트가 초기화되기 때문에, 매번 같은 설명을 반복하지 않기 위해 프로젝트 루트에 규칙을 적어둔 파일을 두고 세션 시작 시 자동으로 읽게 하는 방식을 쓴다. 빌드 명령어, 코딩 컨벤션, 테스트 방법, 아키텍처 결정 같은 내용을 담아두면 에이전트가 매번 같은 실수를 반복하거나 같은 질문을 다시 하는 일을 줄일 수 있다.

문제는 이 컨텍스트 파일이 툴마다 이름과 형식이 제각각이라는 점이다. Claude Code는 `CLAUDE.md`, Codex는 `AGENTS.md`, Cursor는 `.cursor/rules/`(또는 `.cursorrules`), Devin은 `.devin/rules/`, Windsurf는 `.windsurf/rules/`(또는 `.windsurfrules`), Cline은 `.clinerules`, GitHub Copilot은 `.github/copilot-instructions.md`를 읽는다. 🤦‍♂️ 때문에 여러 에이전트를 함께 쓰는 프로젝트라면 같은 내용을 파일마다 중복해서 관리해야 하는 문제가 있다.

이 파편화를 해소하기 위해 나온 것이 `AGENTS.md`다. OpenAI Codex가 처음 사용을 확산시켰고, 현재는 Linux Foundation이 관리하는 벤더 중립적 개방 표준으로 자리잡아 6만 개 이상의 오픈 소스 프로젝트에서 채택되고 있다. 일부 툴은 `AGENTS.md`를 직접 읽지만, 그렇지 않은 툴은 보통 `import`로 불러오거나 심볼릭 링크를 걸어 두 파일을 동기화하는 방식이 실무에서 쓰인다.

[요즘IT의 관련 기사](https://yozm.wishket.com/magazine/detail/3874/?data=EBtJ0K6Ao872zrxGxzYgSVukyhSnTSHfH650J8Yxv1A%3D&source=daily_latest_news)


## 불러오기

### Claude Code

🗓️ 2026-07-31 기준 Claude Code는 아직 자동으로 인식하지 않아, `CLAUDE.md`에서 다음처럼 수동으로 불러온다:

````
@AGENTS.md

## Claude Code
Use plan mode for changes under `src/billing/`.

...

````

심볼릭 링크 방식은 Windows 환경에선 번거로우니 생략.


## 다른 문서 언급하기

그냥 자연어로 쓰면 됨:

````
- 서비스 정책은 `docs/policies/SERVICE-POLICY-2.0.md` 파일 참고
````


## 실제 작성 사례들

- <https://github.com/onflow/flow-go/blob/master/AGENTS.md>
- <https://github.com/promptfoo/promptfoo/blob/main/AGENTS.md>
- <https://github.com/redis/lettuce/blob/main/AGENTS.md>


## 분리 방식

현재(🗓️ 2026-08-26) `AGENTS.md`는 표준 파일명과 Markdown 형식만 정해져 있으며, 문서의 구조나 필수 내용에 대한 규격은 없다.

그래도 만약 본문 내용이 너무 길어서 분리해야 한다면 다음 사례들을 참고할 것:

```bash
# flow-go
.
├── docs/
│   └── agents/
│       ├── CodingConventions.md
│       ├── GoDocs.md
│       └── OperationalDoctrine.md
└── AGENTS.md

# promptfoo
.
├── docs/
│   └── agents/
│       ├── AGENTS.md
│       ├── codex-app-server-provider-no...
│       ├── coding-agent-provider-taxono...
│       ├── database-security.md
│       ├── dependency-management.md
│       ├── git-workflow.md
│       ├── logging.md
│       ├── pr-conventions.md
│       └── python.md
└── AGENTS.md

# lettuce
.
├── .agents/
│   ├── docs/
│   │   ├── api-consistency.md
│   │   ├── architecture.md
│   │   ├── integration-testing.md
│   │   └── javadoc.md
│   ├── rules/
│   │   ├── java-rules.md
│   │   └── rules.md
│   └── skills
└── AGENTS.md
```


## 권장 사항

### 하이퍼링크

```
- 개발 지침: [docs/agents/development.md](docs/agents/development.md)
```

마크다운 링크 형식이 좋다. 어쨋든 `AGENTS.md`도 사람이 읽어야 하는 파일이고, 하이퍼링크로 해석되는 게 편하기 때문. 사실 극한의 효율성을 중시한다면 백틱으로 감싸는 게 더 좋긴 하다. 마크다운 링크 형식은 실제 주소를 분리/해석해야 하는 오버헤드가 발생하기 때문인데, 무시해도 될 수준.

경로 시작 부분에 `/`는 쓰지 않는다. 에이전트 입장에선 프로젝트 루트인지 OS의 루트인지 불확실하기 떄문이다.


## Snippets

### 버전 관리

아예 아무것도 하지 않게 하거나:

````
## Version Control

- Git 관련 작업은 아무것도 하지 말 것
- 특히 사용자 허가 없이 Git 히스토리 조회 금지
````

작업 끝나고 알아서 커밋하게 하거나(🚨 미미하더라도 유료 사용량을 추가로 소모하니 주의):

````
## Version Control

- `code-update-by-agent` 브랜치가 없으면 생성한다.
- 하나의 요청에 해당하는 작업이 완료되면 `code-update-by-agent` 브랜치에 즉시 커밋한다.
- `code-update-by-agent` 브랜치가 current branch보다 뒤에 있는 경우 `code-update-by-agent` 브랜치를 재생성한다.
- 사용자가 rebase를 했을 수 있으므로 `code-update-by-agent`의 커밋이 사라지거나 히스토리가 바뀌어 있어도 정상적인 상황으로 간주한다.
- 커밋 메시지 첫 줄은 `<type>: <short summary>` 형태로 작성하고, `type`은 다음 중 하나를 선택한다: `build, chore, ci, docs, feat, fix, perf, refactor, revert, style, test`
- 나머지 메시지는 한국어로 작성한다.
- 작업 내용은 명사형으로 간결하게 작성한다.
````


## 전체 작성 예시

ℹ️ Next.js 같은 프레임워크가 생성하는 자동 생성 문구는 위치랑 공백, 줄 바꿈까지 그대로 두는 편이 좋다.

#### AGENTS.md

````
# AGENTS.md

## Agent Guidelines

- 개발 지침: [docs/agents/development.md](docs/agents/development.md)
- 디자인/마크업 지침: [docs/agents/design.md](docs/agents/design.md)
- 데이터베이스 지침: [docs/agents/database.md](docs/agents/database.md)

## References

- 서비스 정책: [docs/policies/SERVICE-POLICY-2.0.md](docs/policies/SERVICE-POLICY-2.0.md)

## Hard Rules

- 비즈니스 로직(할인율 계산, 권한 판단, 워크플로우 분기 등)은 애플리케이션 코드에 작성한다. DB 함수(트리거, 저장 프로시저)에 넣지 않는다.
- secret(API key, database password 등)을 클라이언트 레이어에 절대 노출하지 말 것

## Approval Required

다음 작업은 실행 전에 사용자 승인을 받을 것:

- 패키지 설치/삭제
- E2E 테스트 실행
- DB 스키마 변경
- DB 데이터 추가/삭제/변경

## Domain Context

- 일정 예약/관리 시스템
- 핵심 엔티티: 호스트, 게스트, 일정

## Stack

- Next.js + TypeScript + Tailwind
- app: Ionic/Capacitor (추후 개발 기능. 아직 생성하지 말 것)
- database: Supabase
- 배포 플랫폼: Vercel

## Response Style

- Respond concisely.
````

#### docs/agents/design.md

````
# Design & UI Guidelines

## Design Integration

- Claude Design으로 만든 마크업은 `docs/markup/`에 있음
- mockup 디자인 파일은 `docs/design/` 참고

## Static Markup

## Feature Implementation

- 페이지를 구성하는 기능 컴포넌트(`화면 한 블록을 담당하는 컴포넌트)는 최상위 요소에 `data-component="이름"` 속성을 붙인다. 브라우저 Inspect로 컴포넌트 경계와 이름을 바로 확인하기 위한 용도이며, 값은 해당 컴포넌트의 역할을 나타내는 한국어 명사로 작성한다(예: `SiteHeader` → `사이트헤더`, `WeeklySchedule` → `주간일정`).
````

#### docs/agents/development.md

````
# Development Guidelines

## Code Style

## Component

- 기능별 폴더 구조로 분리

## State Management

- Zustand 사용
- 로컬 useState는 단순 UI 상태에만 사용

## API

## Error Handling

## Logging Strategy

- 로그는 구조화된 key-value 형태로 작성한다.
- `DEBUG`: 개발 중 상세한 디버깅 정보에 사용한다.
- `INFO`: 주요 비즈니스 이벤트와 정상적인 작업 완료에 사용한다.
- `WARN`: 처리할 수 있지만 비정상적이거나 주의가 필요한 상황에 사용한다.
- `ERROR`: 요청이나 작업이 실패한 경우에 사용한다.
- 외부 API/DB 호출은 필요할 경우 `durationMs`를 기록한다.
- 서버 로그에는 가능한 경우 `requestId`를 포함한다.
- 비밀번호, 토큰, API Key, 개인정보 등 민감한 정보는 로그에 기록하지 않는다.
- 함수 진입/종료 같은 의미 없는 로그와 과도한 로그는 남기지 않는다.
- 기존 logger와 로깅 패턴이 있다면 그대로 따른다.
````

#### docs/agents/database.md

````
# Database Guidelines

## Supabase Key Management

- anon key 대신 publishable key 사용
- service role key 대신 secret key 사용

## DB Function Policy

- DB 함수는 데이터 무결성 제약(CHECK, FK, 유니크 제약)이나 동시성이 필요한 원자적 연산(예: 잔액 차감)에만 예외적으로 허용한다.
- DB 함수를 추가하기 전에는 애플리케이션 레이어에서 처리할 수 없는 이유를 먼저 확인한다.

## Schema Design

## Naming

## Relationships

## Indexes

## Migrations
````
