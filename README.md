# 내 아이큐 80, 너도 80, 둘이 합쳐 160

#### environments

- [ruby](https://www.ruby-lang.org/ko/)

## 글 작성 도움말

### 주의 사항

- 파일명과 카테고리, 태그는 반드시 소문자로 작성할 것. 파일명과 카테고리는 파일 시스템에 영향을 주기 때문이고 태그는 대소문자 혼용하면 일원화가 힘들다.
- 카테고리는 하나만 작성한다. 여러 깊이의 분류를 구성하는것보다 태그를 여러개 두는 게 낫다.
- 아톰에서 마크다운 syntax가 깨지는 것처럼 보일 때는 반드시 이유가 있다. 정규식이나 언더바 등을 확인할 것.

### kramdown 목차 자동 생성

```html
* Kramdown table of contents
{:toc .toc}
```

### 파일 수정 가이드

#### 주요 scss 파일

- `_sass/simple-texture-home.scss`
- `_sass/simple-texture-blog.scss`
- `_sass/simple-texture/blog/_variables.scss`
- `_sass/simple-texture/blog/_header.scss`
- `_sass/simple-texture/blog/_content.scss`

## 환경 설정

#### RubyGems으로 Bundler와 Jekyll 설치

```bash
gem install bundler jekyll
```

#### 현재 폴더에 사이트 레이아웃 구성하기

```bash
jekyll new . --force
```

만약 현재 폴더에 사이트 레이아웃의 기본 파일들이 있으면 `--force` 옵션으로 덮어쓰게 함.

#### 폴더 및 파일 설명

- `_config.yml`: 설정 파일
- `_posts`: 소스 폴더
- `_site`: 빌드된 결과물
- `.sass-cache`: 몲
- `.jekyll-metadata`: 몲

#### \_config.yml 도움말 보기

- [https://jekyllrb-ko.github.io/docs/configuration/](https://jekyllrb-ko.github.io/docs/configuration/)

## 사이트 빌드

#### 지킬 명령어 도움말 보기

```bash
jekyll help
# 기본 도움말

jekyll build --help
# 명령어별 도움말
```

#### 빌드 방법

```bash
jekyll build
# 현재 폴더의 컨텐츠를 가지고 ./_site 에 사이트를 생성.

jekyll build --drafts
# 초안을 포함하여 빌드. -D 옵션과 같음.

jekyll build --destination <destination>
# 현재 폴더의 컨텐츠를 가지고 <destination> 에 사이트를 생성

jekyll build --watch
# 현재 폴더의 컨텐츠를 가지고 ./_site 에 사이트를 생성.
# 변경사항이 감지되면, 자동으로 다시 생성.
```

#### 로컬 서버 띄우기

```bash
jekyll serve
# 개발서버가 실행됩니다. http://localhost:4000/
# 자동 재생성: 활성화. 비활성화하려면 `--no-watch` 를 사용하세요.

jekyll s -D
# 로컬에 개발서버를 실행하되 draft를 포함.

jekyll serve --livereload
# 변경사항이 발생했을 때 LiveReload 기능이 브라우저를 새로고침합니다.

jekyll serve --incremental
# 재생성 소요시간을 줄이기 위해 증분 재생성 기능으로 부분 빌드를 합니다.

jekyll serve --detach
# `jekyll serve` 와 동일하지만 현재 터미널에 독립적으로 실행됩니다.
# 서버를 종료하려면, `kill -9 1234` 를 실행하세요. "1234" 는 PID 입니다.
# PID 를 모르겠다면, `ps aux | grep jekyll` 를 실행하고 해당 인스턴스를 종료하세요

jekyll serve --no-watch
# `jekyll serve` 와 동일하지만 변경사항을 감시하지 않습니다.
```

#### 로컬 서버 구동

```bash
jekyll serve
# bundle exec jekyll serve
```

로컬 서버에서는 한글 파일명을 인식하지 못하는 문제가 있음.(2018-08-13 기준)

## 지킬 빌드 디버깅 로그

### on 태그는 빌드 불가

```bash
      Remote Theme: Using theme yizeng/jekyll-theme-simple-texture
  Liquid Exception: Liquid error (line 40): comparison of TrueClass with String failed in /_layouts/post.html
             Error: Liquid error (line 40): comparison of TrueClass with String failed
             Error: Run jekyll build --trace for more information.
```
