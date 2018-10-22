# powered by jekyll

## environments
- [ruby](https://www.ruby-lang.org/ko/)

## RubyGems로 Bundler와 Jekyll 설치
```bash
gem install bundler jekyll
```

## 현재 폴더에 사이트 레이아웃 구성하기
```bash
jekyll new . --force
```
만약 현재 폴더에 사이트 레이아웃의 기본 파일들이 있으면 `--force` 옵션으로 덮어쓰게 함.

## 폴더 및 파일 설명
- `_config.yml`: 설정 파일
- `_posts`: 소스 폴더
- `_site`: 빌드된 결과물
- `.sass-cache`: 몲
- `.jekyll-metadata`: 몲

## 환경설정 도움말
- [https://jekyllrb-ko.github.io/docs/configuration/](https://jekyllrb-ko.github.io/docs/configuration/)

## 지킬 명령어 도움말 보기
```bash
jekyll help
# 기본 도움말

jekyll build --help
# 명령어별 도움말
```

## 빌드 방법
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

## 로컬 서버 띄우기
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

## 사이트 빌드
``` bash
jekyll build
```
`./_site` 경로에 정적 사이트 페이지를 생성한다.

## 로컬 서버 구동
```bash
jekyll serve
# bundle exec jekyll serve
```
로컬 서버에서는 한글 파일명을 인식하지 못하는 문제가 있음.(2018-08-13 기준)
