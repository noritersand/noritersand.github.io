# 내 아이큐 80, 너도 80, 둘이 합쳐 160

[![pages-build-deployment](https://github.com/noritersand/noritersand.github.io/actions/workflows/pages/pages-build-deployment/badge.svg?branch=main)](https://github.com/noritersand/noritersand.github.io/actions/workflows/pages/pages-build-deployment)

#### environments

- [ruby](https://www.ruby-lang.org/ko/)
- [jekyll](https://jekyllrb.com/)
- [kramdown](https://kramdown.gettalong.org/)


## 📌 Table of Contents

- [윈도우에서 Jekyll 빌드 환경 설정](#윈도우에서-Jekyll-빌드-환경-설정)
- [사이트 빌드](#사이트-빌드)
- [지킬 빌드 디버깅 로그](#지킬-빌드-디버깅-로그)
- [저장소 내부 링크](#저장소-내부-링크)


## 윈도우에서 Jekyll 빌드

만약 WSL-ubuntu에 빌드 환경을 구성할 거라면 [여기](https://jekyllrb.com/docs/installation/ubuntu/)를 보자. **절대 Quickstart 페이지만 보고 하면 안됨**.

WSL이 아니면 [윈도우용 Ruby + Devkit 설치](https://rubyinstaller.org/downloads/)를 보면 되는데, 각종 버그가 튀어나와서 추천하지 않음.

### RubyGems으로 Bundler와 Jekyll 설치

```bash
gem install bundler jekyll
```

### 의존하는 패키지 설치

```bash
bundler install
```

### 현재 폴더에 사이트 레이아웃 구성하기

**레이아웃 신규 구성하는 거 아니면 안해도 됨**.

```bash
jekyll new . --force
```

만약 현재 폴더에 사이트 레이아웃의 기본 파일들이 있으면 `--force` 옵션으로 덮어쓰게 함.


## 사이트 빌드

한 줄 요약:

```bash
# 빌드 후 로컬 서버 기동
bundle exec jekyll s -DI
```

- `-D` `--drafts`: 원래 빌드하지 않는 경로인 `_drafts` 폴더도 포함해서 빌드.
- `-I` `--incremental`: 서버 기동 중 파일을 수정하면 해당 파일만 다시 빌드하는 옵션. 증분 재생성 옵션이라고 한다.

### 지킬 명령어 도움말 보기

```bash
# 기본 도움말
jekyll help

# 명령어별 도움말
jekyll build --help
jekyll serve --help
```

### 빌드 방법

```bash
# 현재 폴더의 콘텐츠를 가지고 ./_site 에 사이트를 생성.
jekyll build

# 초안을 포함하여 빌드
jekyll build --drafts

# 현재 폴더의 콘텐츠를 가지고 <destination> 에 사이트를 생성
jekyll build --destination <destination>

# 현재 폴더의 콘텐츠를 가지고 ./_site 에 사이트를 생성.
# 변경사항이 감지되면, 자동으로 다시 생성.
jekyll build --watch
```

### 로컬 서버 띄우기

```bash
# 로컬 서버 기동 http://localhost:4000/
jekyll serve

# s는 serve의 단축어
# 로컬서버를 실행하되 draft를 포함.
jekyll s -D

# 변경사항이 발생했을 때 LiveReload 기능이 브라우저를 새로고침
jekyll serve --livereload

# 재생성 소요시간을 줄이기 위해 증분 재생성 기능으로 부분 빌드
jekyll serve --incremental

# jekyll serve 와 동일하지만 현재 터미널에 독립적으로 실행
# 서버를 종료하려면, kill -9 1234 를 실행하세요. "1234" 는 PID
# PID 조회는 ps aux | grep jekyll
jekyll serve --detach

# jekyll serve 와 동일하지만 변경사항을 감시하지 않음.
jekyll serve --no-watch
```

`--livereload`는 켜봤자 빌드하는 동안 404 에러나서 안씀.

`--watch` 옵션은 기본값인 것 같다. (`serve` 실행 중에는 파일 변경사항을 자동으로 감시)

그리고 이유는 모르겠지만 `jekyll`로 시작이 안될때 `bundle exec`로 되기도 함. 아래처럼 해보자:

```bash
gem install bundler # 이미 설치했으면 이건 생략
bundle install # 얘도 했으면 생략
bundle exec jekyll serve
```


## 지킬 빌드 디버깅 로그

WSL에서 빌드하면 대부분 해결되는 것들임.

### 한글 파일명 문제

- 로컬 서버에서는 한글 파일명을 인식하지 못하는 문제가 있는데 딱히 해결 방법을 못 찾음. (2018-08-13)
- WSL에서 띄우니 한글 문제가 사라졌고 3년 만에 모두가 행복해졌다고 한다. (2021-12-23)

### bundler 실행 시 'find_spec_for_exe': can't find gem bundler (>= 0.a) with executable bundle (Gem::GemNotFoundException)

https://github.com/rbenv/rbenv/issues/1138  

아래처럼 `Gemfile.lock`에 있는 버전을 강제로 지정해서 해결함.

```bash
cat Gemfile.lock | grep -A 1 "BUNDLED WITH"
BUNDLED WITH
   1.17.3

gem install bundler -v '1.17.3'
```

### on, off 태그는 빌드 불가

```bash
      Remote Theme: Using theme yizeng/jekyll-theme-simple-texture
  Liquid Exception: Liquid error (line 40): comparison of TrueClass with String failed in /_layouts/post.html
             Error: Liquid error (line 40): comparison of TrueClass with String failed
             Error: Run jekyll build --trace for more information.
```

방법이 없으니 `on`이랑 `off` 태그를 안쓰면 된다.

### Error:  No source of timezone data could be found.

https://jekyllrb.com/docs/installation/windows/#time-zone-management  
윈도우에서 `tzinfo-data` gem 사용 시 발생할 수 있다고 함. `Gemfile` 파일에 아래 추가:

```bash
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw]
```

### Conversion error: Jekyll::Converters::Scss encountered an error while converting 'assets/css/style.scss': Invalid CP949 character "\xE2"

https://jekyllrb.com/docs/installation/windows/#encoding  
지킬 빌드나 서버 구동 시 다국어 관련 에러가 발생할 수 있다. 셸에서 `chcp 65001` 입력 후 다시 실행한다.

### Liquid syntax error: Unexpected character ... {{ ... }}

지킬은 템플릿 언어로 [Liquid](https://shopify.github.io/liquid/basics/introduction/)를 사용하며, Liquid에서 이중 중괄호(double curly braces)`{{ expression }}`는 변수의 값을 출력하는 표현식이다.

이 때문에 글 내용에서 이중 중괄호 코드를 작성하면 Liquid 표현식으로 처리되면서 빌드 오류가 발생하거나 작성한 코드가 안보이거나 하는 현상이 발생한다.

이를 해소하려면 문서 처음 부분과 마지막 부분을 이스케이프하지 않도록 `{% raw %}`와 `{% endraw %}`로 감싸면 된다:

```
(toc)

{% raw %}

(참고한 문서)

(본문)

{% endraw %}

(문서 끝)
```


## 저장소 내부 링크

- [블로그 작성 가이드](docs/guide-and-rules.md)
