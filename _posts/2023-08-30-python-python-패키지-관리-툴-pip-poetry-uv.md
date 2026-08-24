---
layout: post
date: 2023-08-30 17:28:57 +0900
title: '[python] Python 패키지 관리 툴: pip, poetry, uv'
categories:
  - python
tags:
  - python
  - uv
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://www.python.org/](https://www.python.org/)
- [https://pypi.org/project/pip/](https://pypi.org/project/pip/)
- [https://python-poetry.org/](https://python-poetry.org/)

#### 테스트 환경 정보

- Python 3.x
- Pip 22.x
- Poetry 1.x
- uv 0.11.x


## 개요

Python의 패키지 관리 툴 사용방법 정리.


## Pip

Python과 함께 설치되서 따로 설치하지 않아도 됨. 패키지 추가/삭제 정도만 지원한다. (아마도?)

```bash
# 도움말 보기
pip --help

# 버전 조
pip --version

# bs4 패키지 설치
pip install bs4

# 설치된 패키지 정보 출력
pip list

# bs4 패키지 삭제
pip uninstall bs4

# pip 버전 업그레이드
python.exe -m pip install --upgrade pip
```

설치된 패키지 파일 위치는 `C:\Python버전번호\Lib\site-packages` 요 아래에...


## Poetry

패키지 추가/삭제와 스크립트 실행, 가상 환경 생성 같은 기능도 지원한다.

```bash
# 도움말 보기
poetry --help

# 버전 조회
poetry --version

# ⚠️ 명령어 목록 조회. 설치된 패키지 조회가 아님
poetry list

# 설치된 패키지 정보 출력
poetry show

# poetry용 Python 서브프로젝트 생성
poetry new poetry-demo

# bs4 패키지 추가
poetry add bs4

# poetry가 Python 스크립트 실행하게 함
poetry run ../../test/bs.py

# poetry 버전 업그레이드
poetry self update

# 중첩 셸 열기(일종의 가상 실행 환경)
poetry shell
```

설치된 패키지 파일은 `%APPDATA%\pypoetry` 요 아래 어딘가에 있다. 😏


## uv

가상 환경과 패키지 관리를 한 번에 할 수 있다. 사실상 pip와 Poetry, venv가 제공하는 모든 기능을 커버하니 앞으로도 쭉 uv 하나만 써도 된다고 생각될 정도. 실제로 소개 페이지에서 이렇게 설명한다:

> A single tool to replace `pip`, `pip-tools`, `pipx`, `poetry`, `pyenv`, `twine`, `virtualenv`, and more.

### uv 설치

설치는 [choco](https://chocolatey.org/install#individual)로 한다:

```bash
# 관리자 권한 획득 후
choco install uv
```

### 기본 명령어

```bash
# uv 프로젝트 생성
uv init

# 프로젝트 설정과 실제 환경을 동기화: 의존성 설치
uv sync

# 캐시도 새로고침
uv sync --refresh

# 이미 설치되어있건 말건 다시 설치
uv sync --reinstall

# 의존성 트리 표시(프로젝트 선언 기준)
uv tree
```

`uv sync`는 `uv.lock`이나 `pyproject.toml` 파일 기반으로 필요한 패키지를 자동 설치한다. 

### uv init

`uv init`은 `pyproject.toml`과 `src` 레이아웃 등 프로젝트 구조를 스캐폴딩해서 새 파이썬 프로젝트를 초기화하는 명령이다.

자동으로 생성되는 파일 중에 `src/패키지_이름/__init__.py`가 있는데, 작은 스크립트면 `__init__.py`의 `main()` 실행 코드를 작성하면 되지만 이 파일은 원래 패키지 초기화 스크립트이므로 `main.py`를 별도로 생성하는 것이 좋다.

ℹ️ `__init__.py`를 stub 파일(일단 자리만 잡아둔 임시 구현체)이라 부른다.

ℹ️ 상위 디렉터리 이름에 하이픈이 있으면 패키지 이름에서 언더바로 자동 변환된다.

#### 권장하는 파일 구조

상위 디렉터리가 `uv-example`일 때 권장하는 모양은 데충 이렇게 된다:

```
.
└── uv-example/
    ├── .venv
    ├── src/
    │   └── uv_example/
    │       ├── __init__.py
    │       └── main.py
    ├── .gitignore
    ├── .python-version
    ├── pyproject.toml
    └── README.md
```

`__init__.py`:

```py
# 비움
```

`main.py`:

```py
def main() -> None:
    print("Hello from uv-example!");

if __name__ == "__main__":
    main();
```

`pyproject.toml`:

```
[project]
name = "uv-example"

...

[project.scripts]
uv-example = "uv_example:main"

...
```

#### 🚫 `src` 바로 밑에는 `.py` 파일을 두지 않는다.

`src` 바로 밑에는 패키지 폴더만 위치해야 하며, 실제 `.py` 파일들은 그 패키지 폴더 안에 들어가야 한다.

```
src/
  uv_example/          ← ✅ 패키지 폴더
    __init__.py
    main.py
    crawler.py
```

이렇게 두는 것은 맞지 않다.

```
src/
  main.py           ← ❌ 이러면 안 됨
  crawler.py
  uv_example/
    __init__.py
```

이유는 `src/uv_example/` 자체가 하나의 파이썬 패키지이고, `src/`는 그 패키지를 담는 컨테이너 폴더에 불과하기 때문이다. `src` 바로 밑에 `.py` 파일을 두면 `__init__.py`도 없고 패키지로 인식되지 않으므로, `setuptools`나 `hatchling` 같은 빌드 백엔드가 이를 찾지 못한다. `pyproject.toml`의 엔트리포인트(`uv_example.main:main`) 역시 결국 `uv_example` 패키지 내부를 참조하는 것이므로, 구조가 맞지 않으면 빌드나 설치 단계에서 오류가 발생한다.

### uv python

`uv python`은 Python 설치와 버전 관리는 담당하는 하위 명령어다.

```bash
# 설치 가능한 Python 목록과 이미 설치된 목록 표시
uv python list

# Python 3.14 버전 설치
uv python install 3.14

# 특정 버전 삭제
uv python uninstall 3.14

# 설치된 모든 Python의 패치 번호만 업그레이드
uv python upgrade

# .python-version의 버전을 3.14로 설정
uv python pin 3.14

# Python의 설치된 경로 찾기
uv python find

# 설치한 Python 실행 경로를 시스템 환경변수(PATH)에 등록
uv python update-shell
```

### uv add

`uv add`는 패키지를 현재 프로젝트의 의존성으로 선언하고 가상 환경을 찾아 설치까지 한 번에 해주는 명령이다.

```bash
# 의존성 추가하기
uv add PACKAGE_NAME
```

### uv run

`uv run`은 지정한 스크립트나 명령을 프로젝트의 가상환경에서 실행하는 명령이다.

```bash
# 스크립트/명령 실행
uv run FILE_NAME
```

만약 `FILE_NAME`을 `abc`처럼 확장자 없이 지정하면, uv는 관리 중인 가상환경의 `abc`라는 실행 파일이나 명령를 찾아 실행하려고 한다. (이 경우 실행 파일은 보통 `.venv/Scripts` 아래에 `.exe`로 존재함)

### uv cache

캐시 관리 하위 명령어

```bash
# 전체 캐시 삭제
uv cache clean

# 특정 패키지 캐시 삭제
uv cache clean ruff

# 사용되지 않는 캐시와 중앙화된 프로젝트 환경 정리
uv cache prune

# 캐시 위치 확인
uv cache dir
```

### 가상 환경 관리

```bash
# 현재 경로에 가상 환경 생성
uv venv

# 특정 버전의 가상 환경 생성
uv venv --python 3.11

# 가상 환경 내에 패키지 설치
uv pip install fastapi

# 의존성 트리 표시(가상 환경에 설치된 파일 기준)
uv pip tree
```

가상 환경의 실제 위치는 `.venv` 폴더로 확인할 수 있다.

`uv pip install`은 가상 환경 내에 패키지를 '설치만' 한다.

`uv venv`와 `uv pip`는 별도로 가상 환경을 관리하는 게 아니라면 자주 쓰진 않는다.

### pyproject.toml

**TODO**

### uv.lock

**TODO**
