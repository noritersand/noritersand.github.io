---
layout: post
date: 2021-12-22 13:30:31 +0900
title: '[Linux] Docker 도커'
categories:
  - linux
tags:
  - linux
  - docker
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[docker docs\] Getting started](https://docs.docker.com/get-started/)
- [\[서비큐라 기술 블로그\] 초보를 위한 도커 안내서](https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html)

Docker(이하 도커) 관련 정리글.

## Docker Desktop on Windows

윈도우 WSL 환경에서 도커를 사용하고 싶으면 도움말 [\[Microsoft\] WSL 2에서 Docker 원격 컨테이너 시작](https://docs.microsoft.com/ko-kr/windows/wsl/tutorials/wsl-containers)을 보자.

## 이미지

미리 작성된 스크립트로 이해하면 된다. 이미지를 바탕으로 컨테이너를 생성하고 실행한다. 도커 허브를 통해 온라인으로 공유할 수 있음.

## 컨테이너

프로세스들이 돌아가는 격리된 공간. 컨테이너는 이미지를 바탕으로 만들어지며, 하나의 이미지로 여러 컨테이너를 생성할 수 있다.

## 시스템 확인/조회

```bash
# 버전 확인
docker version

# 모든 시스템 관련 정보 확인
docker info

# 이미지 목록 출력
docker images

# 컨테이너의 로그 보기
docker logs CONTAINER_ID

# 컨테이너 목록 보기 #1
docker container ls

# 컨테이너 목록 보기 #2
docker ps

# 컨테이너 목록 보기(실행중이지 않은 컨테이너 포함)
docker ps -a
```

`ps`는 `container ls` 별칭이다. 목록 보기는 실행중인 컨테이너만 표시한다. 다 보려면 `-a` 옵션을 붙여야 함.

## 이미지 검색, 다운로드, 삭제

```bash
# 도커 서버에 IMAGE_NAME 이름으로 등록된 모든 이미지 검색해서 출력
docker search IMAGE_NAME

# IMAGE_NAME 이름으로 등록된 이미지 다운로드
docker pull IMAGE_NAME

# IMAGE_NAME 이미지 삭제
docker rmi IMAGE_NAME

# 강제 삭제
docker rmi IMAGE_NAME -f
```

이미지는 이름 대신 ID를 사용할 수 있고, ID는 앞의 일부만 입력해도 된다.

## 이미지 커밋, 태깅, 업로드

`commit` 명령으로 컨테이너의 스냅샷을 이미지로 저장할 수 있다.

```bash
# 컨테이너 CONTAINER_ID로 my-first-image라는 이미지 생성
docker commit -a 'fixalot' -m 'Hello world!' CONTAINER_ID my-first-image

# my-first-image 이미지에 이름만 fixalot/testbed로 변경한 새 스냅샷 생성
docker tag my-first-image fixalot/testbed

# fixalot/testbed 이미지에 1.0 태그를 붙인 새 스냅샷 생성
docker tag fixalot/testbed fixalot/testbed:1.0

# fixalot/testbed 이미지를 fixalot/testbed 저장소에 업로드
docker push fixalot/testbed
```

`commit`, `tag`, `push` 명령은 이미지명 뒤에 `:tag` 형태로 태그 이름을 지정할 수 있는데 생략하면 `latest`로 설정된다.

`push` 명령은 저장소와 이미지 이름이 일치하지 않으면 실패하는 것 같음.


## 컨테이너 생성과 삭제

컨테이너를 생성하는 명령은 `create`다. 이 명령은 지정한 이미지가 로컬에 없을 경우 도커 서버에서 검색해 자동으로 다운로드(`pull`) 한다.

컨테이너를 생성하면 컨테이너의 고유한 ID와 이름(names)이 자동으로 생성되는데, `start`, `stop`, `kill`, `rm` 명령어는 이 ID나 이름을 지정해야 한다. ID는 앞의 일부만 입력해도 인식하지만 이름은 전체를 명시해야 해서 귀찮다.

```bash
# IMAGE_NAME 이미지로 컨테이너 생성. 만약 이미지가 없으면 자동으로 다운로드
docker create IMAGE_NAME

# 호스트 80, 컨테이너 80 포트로 실행할 컨테이너 생성
docker create -p 80:80 IMAGE_NAME

# CONTAINER_ID 컨테이너 삭제
docker rm CONTAINER_ID
```

## 컨테이너 시작/중단

```bash
# CONTAINER_ID 컨테이너 시작
docker start CONTAINER_ID

# CONTAINER_ID 컨테이너 종료
docker stop CONTAINER_ID

# CONTAINER_ID 컨테이너 중단
docker kill CONTAINER_ID
```

`stop`과 `kill`의 차이는 도움말을 보자:

- [\[docker docs\] docker stop]https://docs.docker.com/engine/reference/commandline/stop
- [\[docker docs\] docker kill]https://docs.docker.com/engine/reference/commandline/kill

대충 검색해보면 안전한 종료(`stop`)인지 안전하지 않은 종료(`kill`)인지의 차이임.

## docker run

`run`은 생성(`create`)하고 시작(`start`)한 뒤 명령어 실행(`exec`)까지 해준다.

```bash
# IMAGE_NAME 이미지로 컨테이너 시작(컨테이너 없으면 생성까지)
docker run IMAGE_NAME

# 백그라운드로 컨테이너 시작
docker run -d IMAGE_NAME

# 프로세스 종료 후 컨테이너 삭제
docker run -rm IMAGE_NAME

# docker/getting-started 이미지가 없으면 다운로드 받은 다음 컨테이너 생성/실행하고
# 호스트 80, 컨테이너 80 포트로 백그라운드(-d)에서 실행
docker run -d -p 80:80 docker/getting-started
```

`--rm` 옵션은 보통 `-it`와 같이 쓰는 모양.

## docker exec

이미 시작된 컨테이너에는 `exec`로 원하는 명령을 실행한다.

```bash
# 백그라운드로 명령어 실행
docker exec -d CONTAINER_ID COMMAND
```

## docker-compose

대충 여러 명령을 한 번에 실행하는 도구인가 봄. `docker-compose.yml` 파일을 미리 만들어야 실행할 수 있다.

[마소에서 아주 잘 설명해놨으니 여기](https://docs.microsoft.com/ko-kr/visualstudio/docker/tutorials/use-docker-compose)를 보자.

```bash
# 버전 확인
docker-compose version

# 실행
docker-compose up
```

## 컨테이너의 터미널 접속하기

생성된 모든 컨테이너는 터미널로 접속할 수 있다. `bash`가 안먹힐 경우 최소한의 기능만 설치된 OS라서 그럴 테니 `sh`로 해보자.

```bash
# IMAGE_NAME 이미지로 컨테이너 생성/시작 후 터미널 접속하며 /bin/bash 실행
docker run -it IMAGE_NAME /bin/bash

# ubuntu 이미지로 컨테이너 생성/시작 후 터미널 접속하며 bash 명령 실행
docker run -it ubuntu bash

# 컨테이너 시작 후 터미널에 접속하며 종료 시 컨테이너 자동 삭제
docker run --rm -it IMAGE_NAME
```

`-i`는 대화형 모드, `-t`는 컨테이너에 입력을 전송하기 위한 옵션임. 대충 터미널로 붙기 위한 옵션이라고 보면 됨.

```bash
# 터미널 접속 가능하도록 IMAGE_NAME 컨테이너 생성
docker create -it IMAGE_NAME

# 컨테이너를 대화형으로 실행
docker start -i CONTAINER_ID

# 컨테이너에 /bin/sh 실행하며 터미널 접속
docker exec -it CONTAINER_ID /bin/sh
```

`start`를 대화형으로 실행하려면 컨테이너를 생성할 때부터 `-it` 옵션을 적용해야 한다:

```bash
$ docker create -it fixalot/testbed bash
6fb8d380f766dfa3532bc1fbf04ffc9ac0ec5176e24d69f53211f3f30648684e

$ docker start -i 6fb8d3
root@6fb8d380f766:/#
```
