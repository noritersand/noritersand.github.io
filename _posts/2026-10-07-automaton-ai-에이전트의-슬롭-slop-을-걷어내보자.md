---
layout: post
date: 2026-10-07 15:16:46 +0900
title: '[Automaton] AI 에이전트의 슬롭(Slop)을 걷어내보자'
categories:
  - automaton
tags:
  - automaton
  - configurations
  - ai-agents
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [쓰레기 코드 ☠️ 없애는 법 \| Nomad Coders](https://www.youtube.com/watch?v=3JCgiVYlLFo)


## 개요

슬롭(slop)이란 AI가 생성한 저품질의 글이나 이미지, 코드 등을 부르기 시작한 말이다.

니콜라스가 이걸 걷어내는 방법을 영상으로 올렸다. 따라해보자.


## Attention Span

[Attention Span](https://github.com/alexgreensh/attention-span)은 코딩 능력은 그대로 두고 답변을 결론부터, 짧게, 훑어보기 쉽게 바꾸는 출력 스타일(output style) 모음이다.

### Claude Code

플러그인을 설치한 뒤 출력 스타일을 고른다.

```bash
# 마켓플레이스 등록
claude plugin marketplace add alexgreensh/attention-span

# 플러그인 설치
claude plugin install --scope user attention-span@attention-span
```

⚠️ 플러그인을 설치만 하면 Claude Code 기본값인 `Default`가 그대로 유지된다. 세 스타일 모두 자동으로 적용하는 `force-for-plugin` 설정이 없어서 직접 골라야 한다.

`/config`의 Output style에서 `Attention-kind`/`Spartan`/`Rundown`을 고르거나, `~/.claude/settings.json`에 기본값으로 지정한다. `outputStyle` 값은 `<플러그인>:<스타일>` 형식으로 쓰고 대소문자도 구분한다. `Attention-kind`처럼 플러그인 이름을 빼거나 철자가 정확히 맞지 않으면 `Default`로 처리된다.

```json
{ 
  "outputStyle": "attention-span:Attention-kind"
}
```

- `Default`: 출력 스타일을 쓰지 않는 상태. Claude Code의 기본 소프트웨어 엔지니어링 지침만으로 답한다.
- `attention-span:Attention-kind`: ADHD(🤔) 친화적인 기본형. 쉬운 말로 결론부터 짧게 답하고, 꼭 필요한 부분만 자세히 푼다.
- `attention-span:Spartan`: 가장 건조한 형태. 결론부터 화살표 목록으로 답하고, 인사말이나 군더더기를 모두 뺀다.
- `attention-span:Rundown`: 브리핑 형태. TL;DR로 시작하고, 진행 상태는 체크박스로, 선택지는 이모지로 표시한다. 작업 현황 보고나 스탠드업에 맞춘 형태다.

세 스타일 모두 `keep-coding-instructions: true`라서 Claude Code의 코딩 지침은 유지하고 답변 형식만 바꾼다.

### 기타 에이전트

스타일 파일에서 Claude Code 전용 frontmatter를 잘라내고 본문만 각 에이전트의 지침 파일에 덧붙인다. 아래는 Codex의 `~/.codex/AGENTS.md`에 넣는 예로, 표시 주석으로 감싸 두면 나중에 업데이트하거나 지우기 쉽다. Antigravity CLI는 `~/GEMINI.md`, Devin은 `~/.codeium/windsurf/memories/`에 넣는다.

```powershell
# PowerShell
$s = irm https://raw.githubusercontent.com/alexgreensh/attention-span/main/output-styles/attention-kind.md
$body = $s -replace '(?s)^.*?<!-- body-start -->[^\n]*\n', ''
Add-Content "$HOME\.codex\AGENTS.md" "`n<!-- attention-span:start -->`n$body<!-- attention-span:end -->"

# Ubuntu
curl -sfL https://raw.githubusercontent.com/alexgreensh/attention-span/main/output-styles/attention-kind.md -o /tmp/attention-span.md \
  && { printf '\n<!-- attention-span:start -->\n'; sed '1,/<!-- body-start -->/d' /tmp/attention-span.md; printf '<!-- attention-span:end -->\n'; } >> ~/.codex/AGENTS.md
```


## Karpathy Skills

[Karpathy Skills](https://github.com/multica-ai/andrej-karpathy-skills)는 LLM이 멋대로 가정하고, 코드를 과하게 추상화하고, 요청과 무관한 곳까지 고치는 버릇을 4가지 원칙으로 막는 `CLAUDE.md` 지침 파일.

### Claude Code

플러그인을 사용자 스코프로 설치한다:

```bash
# 마켓플레이스 등록: 마켓플레이스를 등록하고 스냅샷을 로컬로 내려받는 명령
claude plugin marketplace add forrestchang/andrej-karpathy-skills

# 플러그인 설치
claude plugin install --scope user andrej-karpathy-skills@karpathy-skills
```

⚠️ 이 플러그인은 훅 없이 `karpathy-guidelines` 스킬 하나만 담고 있어서, Claude가 필요하다고 판단할 때만 불러온다. 항상 적용하고 싶으면 아래처럼 `~/.claude/CLAUDE.md`에 덧붙인다:

```powershell
# PowerShell
irm https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md | Add-Content "$HOME\.claude\CLAUDE.md"

# Ubuntu
curl -fsSL https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md >> ~/.claude/CLAUDE.md
```

### 기타 에이전트

Cursor는 저장소의 규칙 파일 `.cursor/rules/karpathy-guidelines.mdc`를 프로젝트의 `.cursor/rules/`에 복사하거나, `skills/karpathy-guidelines/SKILL.md`를 `~/.cursor/skills/`에 넣어 개인 스킬로 쓴다. 그 밖의 에이전트는 `CLAUDE.md` 내용을 각자의 지침 파일에 합친다. 아래는 Codex의 사용자 지침 파일 `~/.codex/AGENTS.md`에 넣는 예다.

```powershell
# PowerShell
irm https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md | Add-Content "$HOME\.codex\AGENTS.md"

# Ubuntu
curl -fsSL https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md >> ~/.codex/AGENTS.md
```


## Superpowers

[Superpowers](https://github.com/obra/superpowers)는 코딩 에이전트용 스킬 모음이자 개발 방법론이다. 바로 코드부터 짜지 않고 브레인스토밍, 설계 승인, 구현 계획, 서브에이전트 기반 구현, TDD, 코드 리뷰, 브랜치 마무리 순서로 진행하게 한다. 아래의 Verification Before Completion도 여기 포함된 스킬이다.

### Claude Code

공식 마켓플레이스에 있으므로 등록 단계 없이 바로 설치한다.

```bash
claude plugin install --scope user superpowers@claude-plugins-official
```

스킬 15개와 SessionStart 훅으로 구성된다. 훅이 세션을 시작하거나 `/clear`, 컨텍스트 압축을 할 때마다 `using-superpowers` 스킬 본문을 컨텍스트에 주입한다. 이 본문이 작업 전에 맞는 스킬부터 찾아 쓰라고 지시하므로, 스킬을 따로 부르지 않아도 상황에 맞춰 불러온다. 그 대가로 켜 두기만 해도 매 세션 일정량의 토큰을 차지한다. 정확한 값은 `claude plugin details superpowers`의 `Always-on`에서 확인한다.

### 기타 에이전트

각 에이전트의 플러그인(확장) 설치 방식으로 설치한다.

```bash
# Codex: /plugins에서 superpowers 검색 후 Install Plugin
# Cursor: 에이전트 채팅에서
/add-plugin superpowers
# Gemini CLI
gemini extensions install https://github.com/obra/superpowers
```

그 밖의 에이전트는 [Superpowers 설치 안내](https://github.com/obra/superpowers#installation)를 참고한다.


## Verification Before Completion

[Verification Before Completion](https://github.com/obra/superpowers/blob/main/skills/verification-before-completion/SKILL.md)은 에이전트가 "다 됐다", "테스트 통과했다"고 말하기 전에 검증 명령을 직접 실행하고 결과를 확인하게 만드는 스킬. Superpowers 스킬 모음에 포함되어 있다.

### Claude Code

Superpowers를 설치했다면 이 스킬도 이미 들어 있다. 설치 방법은 위 [Superpowers](#superpowers) 섹션을 참고한다.

Superpowers는 세션마다 정해진 작업 순서를 따르게 하고, 항상 일정량의 토큰을 차지한다. 이게 부담스러우면 `SKILL.md` 하나만 사용자 스킬 디렉터리에 받아 둔다.

```powershell
# PowerShell
New-Item -ItemType Directory -Force "$HOME\.claude\skills\verification-before-completion" | Out-Null
iwr https://raw.githubusercontent.com/obra/superpowers/main/skills/verification-before-completion/SKILL.md `
  -OutFile "$HOME\.claude\skills\verification-before-completion\SKILL.md"

# Ubuntu
mkdir -p ~/.claude/skills/verification-before-completion
curl -fsSL -o ~/.claude/skills/verification-before-completion/SKILL.md \
  https://raw.githubusercontent.com/obra/superpowers/main/skills/verification-before-completion/SKILL.md
```

### 기타 에이전트

위 [Superpowers](#superpowers) 섹션의 방법으로 Superpowers를 설치하면 이 스킬도 함께 들어온다.


## Anti Slop

[Anti Slop](https://github.com/dmmulroy/anti-slop)은 AI가 흔히 만드는 근거 없는 TypeScript/JavaScript 패턴을 막는 Oxlint 규칙 모음이다. Python은 이를 옮긴 [anti-slop-py](https://github.com/infoslack/anti-slop-py)를 쓴다.

### Claude Code

적용할 프로젝트에서 설치용 에이전트 스킬을 프로젝트에 받은 뒤, Claude에게 anti-slop을 설치해 달라고 요청한다. 스킬이 규칙 소스를 프로젝트에 복사하고 Oxlint 의존성과 lint 설정까지 맞춰 준다.

```bash
# 프로젝트 루트의 셸: Anti Slop 설치 절차가 담긴 스킬을 설치(lint 규칙은 아직 설치되지 않음)
npx skills add dmmulroy/anti-slop --skill install-anti-slop --agent claude-code
```

```bash
# 같은 프로젝트의 Claude Code 세션: 스킬을 실행해 lint 규칙 설치
/install-anti-slop
```

이렇게 `-g` 없이 설치하면 스킬이 프로젝트의 `.agents/skills/`에 들어가고, `.claude/skills/`에서 이를 링크하며, 설치 기록은 `skills-lock.json`에 남는다. 모든 프로젝트에서 쓰려면 `-g`를 붙여 사용자 스코프(`~/.claude/skills/`)에 설치한다.

`install-anti-slop` 스킬은 나중에 업데이트할 때도 쓰이기 때문에 그냥 두는 게 낫다.

### 기타 에이전트

설치 방법은 Claude Code와 같다. `skills` CLI가 Codex, Cursor 등 여러 에이전트에 스킬을 설치해 주므로, 적용할 프로젝트에서 같은 명령으로 스킬을 받은 뒤 에이전트에게 설치를 요청한다. 스킬 없이 직접 하려면 프로젝트의 `src/`를 `tools/oxlint/anti-slop/` 같은 위치에 복사하고 `oxlint.config.ts`의 `jsPlugins`와 `rules`에 등록한다.

```bash
npx skills add dmmulroy/anti-slop --skill install-anti-slop
```

`--agent`를 빼면 이 컴퓨터에서 감지된 에이전트 수에 따라 처리가 달라진다(`skills` 1.7.1 기준).

- 감지된 에이전트가 하나면 묻지 않고 그 에이전트에 설치한다.
- 없거나 둘 이상이면 고르는 화면이 나온다. 이 화면에는 감지된 것만이 아니라 지원하는 에이전트 전체가 나온다.
- `-y`를 붙이거나 AI 에이전트 안에서 실행하면 묻지 않고 감지된 에이전트 전부에 설치한다(감지된 게 없으면 지원하는 에이전트 전체).

원하는 에이전트에만 설치하려면 `--agent codex`처럼 직접 지정한다.


## anti-slop-py

ℹ️ Python 프로젝트에서만 쓰는 규칙 모음.

[anti-slop-py](https://github.com/infoslack/anti-slop-py)는 Anti Slop을 Python으로 옮긴 lint 규칙 모음이다. Ruff에는 아직 사용자 정의 규칙 API가 없어서 Ruff 설정, mypy strict 옵션, Flake8 플러그인 세 도구에 규칙을 나눠 담았다. Pydantic, FastAPI용 규칙 그룹은 선택해서 켠다.

### Claude Code

적용할 프로젝트에서 플러그인을 프로젝트 스코프로 설치한 뒤, 플러그인에 든 설치 스킬을 실행한다. 스킬이 Flake8 플러그인을 `tools/flake8/anti_slop/`에 복사하고 Ruff, mypy 설정을 기존 설정에 합친다. `pydantic`이나 `fastapi`가 직접 의존성이면 해당 규칙 그룹도 켠다.

```bash
# 프로젝트 루트의 셸: 마켓플레이스 등록과 플러그인 설치
claude plugin marketplace add --scope project infoslack/anti-slop-py
claude plugin install --scope project anti-slop-py@anti-slop-py
```

```bash
# 같은 프로젝트의 Claude Code 세션: 스킬을 실행해 lint 규칙 설치
/anti-slop-py:install-anti-slop-py
```

### 기타 에이전트

Anti Slop과 같이 적용할 프로젝트에서 `skills` CLI로 설치용 스킬을 받은 뒤, 에이전트에게 설치를 요청한다.

```bash
npx skills add infoslack/anti-slop-py --skill install-anti-slop-py
```

에이전트 없이 쓰는 방법도 있다.

- 설치 없이 검사만: `uvx --with anti-slop-py flake8 --enable-extensions=ASD,ASF src tests`. 프로젝트에 파일을 하나도 추가하지 않는다.
- 패키지로 설치: `uv add --dev anti-slop-py`(또는 `pip install anti-slop-py`). Flake8에 규칙이 자동 등록되고, Ruff, mypy 설정은 프로젝트의 `configs/`를 참고해 직접 합친다.
