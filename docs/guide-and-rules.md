# 블로그 작성 가이드


## 📌 Table of Contents

- [글 작성 규칙](#글-작성-규칙)
- [환경설정/레이아웃 관련 파일](#환경설정레이아웃-관련-파일)


## 글 작성 규칙

### 기본 규칙

- 탭`\t`은 사용하지 않는다. 들여쓰기는 스페이스로 대체
- 카테고리, 태그는 반드시 소문자로 작성할 것. 파일명과 카테고리는 파일 시스템에 영향을 주기 때문이고 태그는 대소문자 혼용하면 같은 태그인데도 이원화됨.
- 태그는 가급적 단수형으로 작성하며, 띄어쓰지 않는다. 구분이 필요하면 `-` 사용.
- 카테고리와 일치하는 태그는 하나씩 있어야 함.
- 카테고리는 하나만 작성한다. 여러 깊이의 분류를 구성하는것보다 태그를 여러개 두는 게 낫다.
- 아톰에서 마크다운 syntax가 깨지는 것처럼 보일 때는 반드시 이유가 있다. 정규식이나 언더바 등을 확인할 것.
- 리스트를 나열할 때는 콜론이 앞에 있는게 맞다. 단, 리스트의 제목으로 H4 헤더를 사용할 경우는 제외.
- 헤더는 H2, H3, H4만 사용한다. 현재 적용한 블로그 레이아웃에서 H1은 글 제목으로 쓰이고 있고 H5, H6은 본문보다 작다.
- 헤더에는 불가피한 경우를 제외 코드 블록을 사용하지 않는다.
- H1 혹은 H2 태그는 이전 내용이 H3나 H4였을 때 두 줄을 비우고 작성한다. 안그러면 마크다운 파일을 직접 읽을 때 가독성이 너무 떨어짐.
- 화살표는 `->` 로 작성한다. (JS 화살표 함수랑 겹치니께...)
- 한 문서 내에서의 링크는 `#heading-제목`으로 링크. (e.g. [들여쓰기](#heading-들여쓰기))
- 다른 문서로의 링크는 [이런식](/카테고리/날짜와-확장자를-제외한-파일명/)으로 한다. 당연히 파일명이 바뀌면 링크도 깨짐.

### 하이퍼링크

- API 항목별로 공식 도움말 링크를 거는 짓은 하지 말자. 주소가 바뀌면 노가다 가능성이 있고 공식 도움말 직접 찾아가는 건 일도 아니다.
- 굳이 하고 싶으면 '참고한 문서' 항목에 카테고리 성격의 페이지만 추가할 것

### 글 제목

- `[카테고리] 글 제목` 형태로 작성.
- 에디터에서 찾아야 하기 때문에 파일명과 반드시 일치할 필요 음슴.
- 제목은 가급적 한글만 쓴다. (어차피 파일명에 영문 들어감)
- 카테고리는 세부화하는 게 좋으려나? 🤔 가령 Vue.js는 JavaScript로 해야할 지

### 파일명

- 파일명은 글 제목의 특수기호`[] () ,`를 제거하고 공백을 하이픈`-`으로 대체한 뒤 소문자로 작성한다.
- **파일명은 한글과 영문을 병기한다. 이렇게 하면 fuzzy finder로 찾기 수월하고, 검색 엔진 최적화에도 눈꼽만큼 도움됨.**

### 들여쓰기

- 스페이스바2: javascript, shell script
- 스페이스바4: java, python, xml

### kramdown 목차 자동 생성

```html
* Kramdown table of contents
{:toc .toc}
```

### 코드

- 중괄호`{}`와 대괄호`[]` 다음은 띄어쓰지 않는다. 단, syntax 혹은 문법의 모양을 보여줄 때는 띄어씀.

#### ℹ️ 인라인 코드로 작성해야 하는 것들

- 타입: 프로토타입 혹은 클래스
- TODO

### 명령어 옵션이나 키보드 단축키를 표현할 때

or 조건, 즉 나열된 것 중 아무거나 해도 되는걸 표현할 땐 공백을 사용한다. 

예를 들어 옵션은 이렇게:

- `-n` `--no-commit`: 설명
- `-` `-l` `--login`: 설명

키보드 단축키는 이렇게:

- <kbd>win + t</kbd> <kbd>win + shift + t</kbd>: 설명
- <kbd>f5</kbd> <kbd>ctrl + g</kbd>: 설명

하면 됨.


## 환경설정/레이아웃 관련 파일

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
- ~~ctrl + k~~ 이건 추가했다가 문제가 많아서 롤백 (파폭 단축키와 겹침)

### scss 파일

- `_sass/simple-texture-home.scss`
- `_sass/simple-texture-blog.scss`
- `_sass/simple-texture/blog/_variables.scss`
- `_sass/simple-texture/blog/_header.scss`
- `_sass/simple-texture/blog/_content.scss`
