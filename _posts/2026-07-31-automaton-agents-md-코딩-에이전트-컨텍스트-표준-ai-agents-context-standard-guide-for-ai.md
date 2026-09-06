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

### 링크

````
1. 개발/구현 지침: [docs/agents/development.md](docs/agents/development.md) ❌
2. [개발/구현 지침](docs/agents/development.md) ❌
3. 개발/구현 지침: `docs/agents/development.md` ✅
````

사람을 위한 문서라면 1번이 HTML로 렌더링된 상태에서 주소까지 보이고 하이퍼링크로 작동되서 가장 좋다.

하지만 지침 문서는 에이전트가 몇 번이고 읽어야 하는 파일이니 3번이 낫다. 마크다운 링크는 코드에 비해 실제 주소를 분리/해석하는 오버헤드가 발생한다.

경로 시작 부분에 `/`는 쓰지 않는다. 에이전트 입장에선 프로젝트 루트인지 OS의 루트인지 불확실하기 떄문이다.


## 작성 예시

ℹ️ Next.js 같은 프레임워크가 생성하는 자동 생성 문구는 위치랑 공백, 줄 바꿈까지 처음 작성된 그대로 두는 편이 좋음

````
# AGENTS.md

## Agent Guidelines

- 개발/구현 지침: `docs/agents/development.md`
- 디자인/마크업 지침: `docs/agents/design.md`
- 데이터베이스 지침: `docs/agents/database.md`

## Hard Rules

- 비즈니스 로직(할인율 계산, 권한 판단, 워크플로우 분기 등)은 애플리케이션 코드에 작성한다. DB 함수(트리거, 저장 프로시저)에 넣지 않는다.
- secret(API key, database password 등)을 클라이언트 레이어에 절대 노출하지 말 것

...

````
