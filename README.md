# hexo branch for generating html

## environments
- node.js
- npm

## 배포 설정
`hexo-deployer-git`이 배포를 같은 저장소의 master 브랜치로 하게 되어 있음.

## 클론 후 필요한 최초 설정
```bash
npm install
npm install -g hexo-cli
hexo d -g  # generate 후 deploy
```

## 글 작성
```bash
hexo new 파일명
```

## 초안(비공개글)으로 작성하기
```bash
hexo new draft 파일명
```

## 로컬서버에서 초안 확인
`--draft` 옵션을 사용해서 서버를 띄우면 초안 확인 가능함.
```bash
hexo server --draft
```

## draft를 공개글로 전환
```bash
hexo publish 파일명
```
파일명 앞의 일부분만 명시해도 인식함. 

## 글 수정
`source\_posts`, `source\_drafts` 폴더 아래의 md 파일을 직접 수정

## builder와 site의 싱크가 안맞을 떄
public 폴더를 삭제하고 다시 generate하면 된다. (특히 대소문자 변경했을 떄)
