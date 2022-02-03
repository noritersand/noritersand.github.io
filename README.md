# 내 아이큐 80, 너도 80, 둘이 합쳐 160

#### environments

- [ruby](https://www.ruby-lang.org/ko/)
- [jekyll](https://jekyllrb.com/)
- [kramdown](https://kramdown.gettalong.org/)

## 글 작성 가이드

### 작성 규칙

#### 기본 규칙

- 탭`\t`은 사용하지 않는다. 들여쓰기는 스페이스로 대체
- 카테고리, 태그는 반드시 소문자로 작성할 것. 파일명과 카테고리는 파일 시스템에 영향을 주기 때문이고 태그는 대소문자 혼용하면 같은 태그인데도 이원화됨.
- 태그는 가급적 단수형으로 작성하며, 띄어쓰지 않는다. 구분이 필요하면 `-` 사용.
- 카테고리와 일치하는 태그는 하나씩 있어야 함.
- 카테고리는 하나만 작성한다. 여러 깊이의 분류를 구성하는것보다 태그를 여러개 두는 게 낫다.
- 아톰에서 마크다운 syntax가 깨지는 것처럼 보일 때는 반드시 이유가 있다. 정규식이나 언더바 등을 확인할 것.
- 헤더는 H2, H3, H4만 사용한다. 현재 적용한 블로그 레이아웃에서 H1은 글 제목으로 쓰이고 있고 H5, H6은 본문보다 작다.
- 리스트를 나열할 때는 콜론이 앞에 있는게 맞다. 단, 리스트의 제목으로 H4 헤더를 사용할 경우는 제외.
- 헤더에는 불가피한 경우를 제외 코드 블럭을 사용하지 않는다.
- 화살표는 `->` 로 작성한다. (JS 화살표 함수랑 겹치니께...)
- 한 문서 내에서의 링크는 `#heading-제목`으로 링크. (e.g. [들여쓰기](#heading-들여쓰기))

#### 파일명

- 파일명은 글 제목의 특수기호`[] () ,`를 제거하고 공백을 하이픈`-`으로 대체한 뒤 소문자로 작성한다.
- fuzzy finder로 찾기 쉽도록 글 제목과 다른 단어를 추가할 수도 있음. (basics 라던지...)
- TODO 글 제목이 중구난방이라서 규칙 더 필요. 특히 우리말과 영어를 병기하는 부분이 지멋대로임.

#### 들여쓰기

- 스페이스바2: javascript, shell script
- 스페이스바4: java, python, xml

### kramdown 목차 자동 생성

```html
* Kramdown table of contents
{:toc .toc}
```

## 환경설정 및 레이아웃 관련 파일

- `_config.yml`: 설정 파일
- `_posts`: 소스 폴더
- `_site`: 빌드된 결과물
- `.sass-cache`: 몲
- `.jekyll-metadata`: 몲

### \_config.yml 도움말

- [https://jekyllrb-ko.github.io/docs/configuration/](https://jekyllrb-ko.github.io/docs/configuration/)

### post 템플릿

- `_layouts/post.html`

### 검색 관련 파일

- `assets\javascripts\jekyll-search.jquery.js`
- `_includes\blog\scripts.html`

#### 검색창 열기 단축키

- <kbd>ctrl + p</kbd>
- <kbd>ctrl + /</kbd>
- <kbd>shift + /</kbd>

### 주요 scss 파일

- `_sass/simple-texture-home.scss`
- `_sass/simple-texture-blog.scss`
- `_sass/simple-texture/blog/_variables.scss`
- `_sass/simple-texture/blog/_header.scss`
- `_sass/simple-texture/blog/_content.scss`

## 윈도우에서 Jekyll 빌드 환경 설정

만약 WSL-ubuntu에 설치할거면 [여기](https://jekyllrb.com/docs/installation/ubuntu/)를 보자. **절대 Quickstart 페이지만 보고 하면 안됨**.

WSL이 아니면 [윈도우용 Ruby + Devkit 설치](https://rubyinstaller.org/downloads/).

### RubyGems으로 Bundler와 Jekyll 설치

```bash
gem install bundler jekyll
```

### 의존하는 패키지 설치

```bash
bundler install
```

### 현재 폴더에 사이트 레이아웃 구성하기

**블로그 레이아웃을 처음 만드는 거 아니면 안해도 됨**.

```bash
jekyll new . --force
```

만약 현재 폴더에 사이트 레이아웃의 기본 파일들이 있으면 `--force` 옵션으로 덮어쓰게 함.

## 사이트 빌드

미리 요약하면 빌드와 로컬 서버 기동은 아래 한 줄로 됨:

```bash
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
# 현재 폴더의 컨텐츠를 가지고 ./_site 에 사이트를 생성.
jekyll build

# 초안을 포함하여 빌드
jekyll build --drafts

# 현재 폴더의 컨텐츠를 가지고 <destination> 에 사이트를 생성
jekyll build --destination <destination>

# 현재 폴더의 컨텐츠를 가지고 ./_site 에 사이트를 생성.
# 변경사항이 감지되면, 자동으로 다시 생성.
jekyll build --watch
```

### 로컬 서버 띄우기

```bash
# 개발서버가 실행됩니다. http://localhost:4000/
# 자동 재생성: 활성화. 비활성화하려면 `--no-watch` 를 사용하세요.
jekyll serve

# 로컬서버를 실행하되 draft를 포함.
jekyll s -D

# 변경사항이 발생했을 때 LiveReload 기능이 브라우저를 새로고침합니다.
jekyll serve --livereload

# 재생성 소요시간을 줄이기 위해 증분 재생성 기능으로 부분 빌드를 합니다.
jekyll serve --incremental

# `jekyll serve` 와 동일하지만 현재 터미널에 독립적으로 실행됩니다.
# 서버를 종료하려면, `kill -9 1234` 를 실행하세요. "1234" 는 PID 입니다.
# PID 를 모르겠다면, `ps aux | grep jekyll` 를 실행하고 해당 인스턴스를 종료하세요
jekyll serve --detach

# `jekyll serve` 와 동일하지만 변경사항을 감시하지 않습니다.
jekyll serve --no-watch
```

`--livereload`는 켜봤자 빌드하는 동안 404 에러나서 안씀.

`--watch` 옵션을 생략해도 `serve` 실행 중에는 파일 변경사항을 자동으로 감시하는 것 같음.

그리고 이유는 잘 모르겠지만 `jekyll`로 시작이 안될때 `bundle exec`로 되기도 함. 아래처럼 해보자:

```bash
gem install bundler # 이미 설치했으면 이건 생략
bundle install # 얘도 했으면 생략
bundle exec jekyll serve
```

## 지킬 빌드 디버깅 로그

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

### on 태그는 빌드 불가

```bash
      Remote Theme: Using theme yizeng/jekyll-theme-simple-texture
  Liquid Exception: Liquid error (line 40): comparison of TrueClass with String failed in /_layouts/post.html
             Error: Liquid error (line 40): comparison of TrueClass with String failed
             Error: Run jekyll build --trace for more information.
```

방법이 없으니 `on` 태그를 안쓰면 된다.

### Error:  No source of timezone data could be found.

https://jekyllrb.com/docs/installation/windows/#time-zone-management  
윈도우에서 `tzinfo-data` gem 사용 시 발생할 수 있다고 함. `Gemfile` 파일에 아래 추가:

```bash
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw]
```

### Conversion error: Jekyll::Converters::Scss encountered an error while converting 'assets/css/style.scss': Invalid CP949 character "\xE2"

https://jekyllrb.com/docs/installation/windows/#encoding  
지킬 빌드나 서버 구동 시 다국어 관련 에러가 발생할 수 있다. 쉘에서 `chcp 65001` 입력 후 다시 실행한다.
