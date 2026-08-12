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

가상 환경과 패키지 관리를 한 번에 할 수 있다. 사실상 pip와 Poetry, venv가 제공하는 모든 기능을 커버하니 앞으로도 쭉 uv 하나만 써도 된다고 생각될 정도.

### uv 설치

설치는 [choco](https://chocolatey.org/install#individual)로 한다:

```bash
# 관리자 권한 획득 후
choco install uv
```

### 기본 사용 방법

```bash
# uv 프로젝트 생성
uv init

# 의존성 설치하기
uv sync

# 의존성 추가하기
uv add PACKAGE_NAME

# Python 스크립트 실행
uv run FILE_NAME

# 의존성 트리 표시(프로젝트 선언 기준)
uv tree
```

`sync`는 `uv.lock`이나 `pyproject.toml` 파일 기반으로 필요한 패키지를 자동 설치한다. 

`run`은 기본적으로 uv가 가상 환경을 만들거나 찾아서 그 안에서 스크립트를 실행한다.

`add`는 패키지를 현재 프로젝트의 의존성으로 선언하고 가상 환경을 찾아 설치까지 한 번에 해준다.

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

`pip install`은 가상 환경 내에 패키지를 '설치만' 한다.

`venv`와 `pip`는 별도로 가상 환경을 관리하는 게 아니라면 자주 쓰진 않는다.
