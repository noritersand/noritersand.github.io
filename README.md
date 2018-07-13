# site builder by jekyll

## environments
- ruby

## 설정 파일
- \_config.yml

## RubyGems로 Bundler와 Jekyll 설치
```bash
gem install bundler jekyll
```

## 현재 폴더에 사이트 레이아웃 구성하기
```bash
jekyll new . --force
```
만약 현재 폴더에 사이트 레이아웃의 기본 파일들이 있으면 `-f` 옵션으로 덮어쓰게 함.

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

## 글 작성
```bash

```
