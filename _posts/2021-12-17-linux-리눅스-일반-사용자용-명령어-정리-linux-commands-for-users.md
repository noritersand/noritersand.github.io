---
layout: post
date: 2021-12-17 15:19:13 +0900
title: '[Linux] 리눅스 일반 사용자용 명령어 정리'
categories:
  - linux
tags:
  - linux
  - os
  - user
  - commands
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [Official Ubuntu Documentation](https://help.ubuntu.com/)
- [https://wiki.kldp.org/HOWTO/html/Adv-Bash-Scr-HOWTO/](https://wiki.kldp.org/HOWTO/html/Adv-Bash-Scr-HOWTO/)

#### 버전 정보

- 대부분의 명령은 Ubuntu 20.x 기준으로 작성됨

## 개요

리눅스 명령어 간단 정리 글. 항상 루트 권한이 필요한 명령은 다른 글로.

## Ubuntu의 터미널 단축키

### 캐럿 이동, 삭제

- <kbd>ctrl + e</kbd>: 캐럿을 오른쪽 끝으로
- <kbd>ctrl + a</kbd>: 캐럿을 왼쪽 끝으로
- <kbd>ctrl + left</kbd>: 왼쪽의 다음 단어로 점프
- <kbd>ctrl + right</kbd>: 오른쪽의 다음 단어로 점프
- <kbd>alt + backspace</kbd>: 왼쪽의 단어 삭제.
- <kbd>ctrl + delete</kbd>: 오른쪽의 단어 삭제.

삭제할 땐 클립보드 저장까지 됨. 구버전에선 진짜 삭제만 했지 않았을까?

### 잘라내기, 붙여넣기

- <kbd>ctrl + w</kbd>: 왼쪽의 단어 잘라내기. 클립보드로 저장됨.
- <kbd>ctrl + y</kbd>: 클립보드 붙여넣기
- <kbd>ctrl + k</kbd>: 현재 캐럿 기준 오른쪽 끝까지 잘라내기
- <kbd>ctrl + u</kbd>: 캐럿을 라인의 맨 처음으로 이동하며 모두 잘라내기
- <kbd>ctrl + l</kbd>: 화면 지우기. 버퍼에는 남아있어서 위로 올리면 보임

## 문법

### | (파이프)

둘 이상의 명령어를 연결

### > , >> , < (리디렉션)

```bash
명령어 > 파일  # 파일이 없으면 생성하고, 있으면 기존내용을 지움
명령어 >> 파일  # 파일이 없으면 생성하고, 있으면 기존내용을 추가
명령어 < 파일  # 파일의 표준입력을 받는다
```

## command

명령어가 존재하는지 확인.

```bash
command -v COMMAND_NAME
```

## ssh

secure shell 터미널 연결

```
ssh user@]host[:port]
```

```bash
# PRIVATE_KEY_FILE.pem을 개인키로 사용하며 SERVER_IP서버 포트번호 22번에 접속
ssh -i ./PRIVATE_KEY_FILE.pem ubuntu@SERVER_IP -p 22
```

키가 public 상태면 접속이 안될 수 있으니 이 때는 600 권한 주면 됨:

```bash
chmod 600 ./PRIVATE_KEY_FILE.pem
```

## sftp

ssh와 옵션은 거의 같음.

```bash
# PRIVATE_KEY_FILE.pem을 개인키로 사용하며 서버에 접속, ~/repo/a.txt를 로컬의 ~/downloads 경로로 다운로드
sftp -i ./PRIVATE_KEY_FILE.pem ubuntu@1.2.3.4:repo/a.txt ~/downloads
```

## grep

패턴 검색

```
grep [OPTIONS] PATTERN [FILE...]
grep [OPTIONS] [-e PATTERN | -f FILE] [FILE...]
```

#### options

- `-r`: 지정된 대상이 디렉터리일 경우 하위의 파일을 검색
- `-c`: 패턴이 일치하는 줄의 수를 출력
- `-i`: 비교시 대소문자를 구별 안함
- `-v`: 지정한 패턴과 일치하지 않는 줄만 출력
- `-n`: 줄 번호를 함께 출력
- `-l`: 패턴이 포함된 파일의 이름을 출력
- `-w`: 패턴이 전체 단어와 일치하는 줄만 출력
- `-E`: 정규식 표현으로 검색
- `-f`: 파일의 내용을 검색하는... 것 같다.

옵션을 생략할 경우 현재 경로 대상이며 패턴이 검색된 파일명과 해당 줄의 문자열 출력한다.

```bash
# httpd.conf 파일에서 'httpd' 패턴을 찾아 줄 번호와 문자열 출력
grep -n 'http' httpd.conf

# /web 경로 아래에서 'abcd' 재귀검색
grep -nr 'abcd' /web/*

# /userhome 아래의 모든 경로에서 'error' 패턴을 검색해 파일명만 출력
grep -lr 'error' /userhome/*

# 모든 파일 중 'error' 패턴이 검색되지 않는 파일명만 출력
grep -lv 'error' *
```

### 출력 필터링

```bash
# httpd.conf 파일 중 root가 포함된 줄만 출력
cat ./httpd.conf | grep root

# access_log 파일의 변화를 추적하되 'error'가 포함된 줄만 출력
tail -f access_log | grep error

# 'd'로 시작하는 줄만 출력
ls -l | grep '^d'
```

파이프`|` 이후의 grep은 파이프 이전의 명령으로 발생하는 출력을 필터링한다는 의미다. 아래처럼 옵션도 사용 가능하다:

```bash
git status | grep -v deleted # 'deleted'가 포함되지 않은 줄만 출력
```

#### grep AND, OR, NOT

```bash
# AND: 'Abc', '123', 'xyz'가 모두 포함된 라인만 출력
ls -l | grep Abc | grep 123 | grep xyz
ls -l | grep -E 'abc.*def'

# OR: 'abc' 혹은 'def'가 포함된 라인만 출력
ls -l | grep -e abc -e def
ls -l | grep -E 'abc|def'

# NOT: 'xyz'가 포함된 라인은 제외하고 출력
ls -l | grep -v 'xyz'
```

## find

파일 찾기. 이 명령어는 `-maxdepth` 옵션을 따로 지정하지 않으면 재귀적으로 작동한다.

```bash
find ./ -name 'log' # 이름이 'log'인 모든 파일과 디렉터리를 현재 경로와 하위에서 검색
find ./ -name '*.log' -type f # 확장자가 'log'인 모든 파일만 현재 경로와 하위에서 검색
find -name 'a' -o -name 'b' # 'a'와 'b' 같이 찾기
find ~ ! -name 'sample' # 지정된 경로에서 'sample'을 제외한 모든 파일과 디렉터리 찾기
```

## which

현재 환경 변수 상 지정한 명령어의 실행파일이 어느 경로에 있는지 출력한다.

```bash
# vim 경로 출력
which vim

# vim 검색 가능한 모든 경로 출력
which vim

# vim과 vi의 경로 출력
which vim vi
```

## whereis

명령 실행을 위한 이진파일, 소스 혹은 메뉴얼 파일들이 어느 경로에 있는지 출력한다.

```bash
# ls 명령 관련 파일들의 경로 모두 출력
whereis ls
```

## curl

지정한 경로로 HTTP 요청을 전송한다.

```bash
# plain get request
curl http://tistory.com

# 도메인 localhost 포트번호 4000번에 GET 요청하되 응답 헤더도 출력
curl -i localhost:4000

# post request with header
curl -X POST "http://take-my-json-attack.com" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"prdId\": \"123456\"}"
```

## wget

컨텐츠 다운로드 명령어. 지정된 URL의 결과를 파일로 다운로드한다.

```bash
# example.com의 "index.html" 제목 페이지를 파일로 내려받기
wget http://www.example.com/

# GNU FTP 사이트로부터 Wget의 소스 코드 내려받기.
wget ftp://ftp.gnu.org/pub/gnu/wget/somefile
```

## rm/rmdir

파일 혹은 디렉터리 삭제

```
rm 대상파일경로
rmdir 대상디렉터리경로
```
```bash
rm -rf ./trash # trash 디렉터리 내의 모든 파일과 하위 디렉터리를 프롬프트 없이 삭제
rm -f ./some-link # 심볼릭 링크 삭제
rmdir ./test-folder # 비어있는 디렉터리 삭제
```

## ln

바로가기를 생성한다.

```
ln target
```

#### options

- `-s`: 하드 링크 대신 심볼릭 링크를 생성한다.

```bash
ln -s ~/was/upload ./upload
# 현재 경로에 upload라는 심볼릭 링크를 생성한다. 이 링크의 실제 물리경로는 home/was/upload
```

## top

리눅스 커널(kernel)이 관리중인 프로세스(혹은 쓰레드) 목록과 CPU/메모리 점유율, PID 등을 실시간으로 출력한다. 아무 옵션도 지정하지 않으면 CPU 점유율 내림차순, PID 오름차순으로 보여줌.

``` bash
top -o cpu  # cpu 자원을 가장 많이 점유하고 있는 프로세스순 보기
```

## ps

실행중인 프로세스를 확인하는 명령어. 프로세스들의 정보를 스냅샷으로 출력한다.

```bash
ps -ef
ps -eF
ps -eLf
ps -eLF
ps aux | grep -e manage.py | grep -v grep
```

## kill

실행중인 프로세스를 죽인다(?).

```
kill 프로세스아이디
```

## pkill

프로세스를 이름으로 죽인다.

```bash
pkill -9 -ef catalina
```

#### options

- `-9`: kill을 의미
- `-e`: 로그 출력
- `-f`: 명령줄 전체 참조 옵션

## nohup

프로세스를 백그라운드로 실행

[http://changpd.blogspot.kr/2013/04/linux-nohup-xxxsh.html](http://changpd.blogspot.kr/2013/04/linux-nohup-xxxsh.html)

``` bash
nohup java -jar example.jar &
```

`&` 만 쳐도 됨.

## cd

명령줄의 경로 이동

```bash
cd /  # 루트(최상위 기본) 경로로 이동
cd ~  # 로그인 유저의 홈으로 이동
cd ..  # 한단계 상위 경로로 이동
cd /bin/lib  # bin 하위의 lib으로 이동
```

## ls

해당 디렉터리 내부 리스트 보기

```bash
# 파일/폴더 보기
ls

# 숨김 파일 포함 리스트형식으로 표시하되 용량은 기가바이트 단위로
ls -al --block-size=G

# 숨긴 파일 + 리스트형식 + 인디케이터 표시
ls -alF

# -t의 역순(이 경우 수정시각 기준 오름차순) 정렬
ls -tr

# 리스트 + 수정시각 오름차순 + inode 표시
ls -ltri

# 리스트 + 디렉터리만 출력
ls -ld */
```

#### options

- `-l`: 리스트 형태로 보기
- `-a`: 숨겨진 파일까지 모두 보기
- `-t`: 마지막 수정시각 기준으로 내림차순 정렬
- `-r`: 역으로 정렬
- `-i`: inode 번호 표시
- `-F` `--classify`: 파일의 종류에 따라 파일이나 디렉터리 이름 뒤에 인디케이터(`*/=>@|` 중에 하나)를 붙임. 가령 `abc*`로 보이는건 `abc` 파일이 실행파일이란 의미다.

## readlink

심볼릭 링크 혹은 캐노니컬(canonical) 파일 이름을 출력한다.

```bash
# FILE_NAME 파일의 전체 경로 + 파일명 출력
readlink -f FILE_NAME
```

## cat

파일 내용 출력

```bash
cat READ_ME  # READ_ME 파일 내용을 화면에 출력
```

## head

```bash
# filename의 처음부터 100줄만 출력
head -n 100 filename
head -100 filename
```

## [tail](https://man7.org/linux/man-pages/man1/tail.1.html)

```bash
# filename의 끝부터 100줄만 출력
tail -n 100 filename
tail -100 filename
```

#### options

- `-n` `--lines=[+]NUM`: 특정 줄 번호부터 출력

## head와 tail의 혼합

```bash
head -n 20 filename | tail -n 10 # 처음부터 20줄 중 마지막 10줄만 출력
tail -5 filename | head -3 # 마지막 다섯 줄 중 처음 세 줄만 출력
```

## sed

?

```bash
sed -n '2,5p' filename # filename에서 2~5라인만 출력
```

## more

파일 내용을 화면단위로 분할하여 출력. 엔터로 한 줄, 스페이스바로 한 페이지씩 넘긴다.

```bash
more READ_ME
```

## [less](https://man7.org/linux/man-pages/man1/less.1.html)

파일 뷰어. vi 읽기전용 모드와 비슷하지만 메모리 점유는 적다.

```bash
less READ_ME
less -N READ_ME # 행 번호 표시
less +7 READ_ME # READ_ME 파일의 7번째 줄 번호부터 출력()

# ls의 도움말을 less로 보기
ls --help | less
```

#### options

- `-n` `--line-numbers`: 줄 번호 표시 생략.
- `-N` `--LINE-NUMBERS`: 줄 번호 표시.
- `+`: 숫자와 이어붙여 사용하며, 특정 줄 번호를 시작위치로 지정함.

## vi

VIM 파일 편집기.

```bash
vi READ_ME # 해당경로에 READ_ME 파일 편집모드 (파일 없을경우 생성하며 편집모드)
vi -R READ_ME # 읽기 전용 모드로 열기
view READ_ME # -R 옵션이 view 명령으로 지정된 vim 버전도 있음.
```

## cp

파일 복사

```bash
cp READ_ME testfolder  # (내부경로) READ_ME 파일을 testfolder 디렉터리 내부에 복사
cp -a ... ...  # 파일의 소유권과 각종 정보 유지하여 복사 (위의 경우는 유지안됨)
```

**주의**: 파일 복사나 이동은 대상 경로의 존재여부에 따라 결과가 달라진다. [이에 대해 잘 정리된 문서 링크](https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4%EC%97%90%EC%84%9C_%ED%8F%B4%EB%8D%94_%ED%86%B5%EC%A7%B8%EB%A1%9C_%EB%B3%B5%EC%82%AC%ED%95%98%EA%B8%B0).

## mkdir

디렉터리 생성

```bash
# 현재 경로에 test 디렉터리 생성
mkdir test  

# 경로 /home/noritersand/apache2 에 디렉터리를 생성하면서 필요한 경우 부모 디렉터리도 같이 생성
mkdir -p /home/noritersand/apache2
```

## mv

디렉터리와 파일명 변경(이동)한다.

```bash
mv test test2  # test -> test2 디렉터리명 변경
mv READ_ME test2.doc  # READ_ME -> test.doc 파일명 변경
mv READ_ME ./testfolder/  # READ_ME 파일 testfolder 디렉터리내로 이동
mv READ_ME ./testfolder/READ_ME2  # READ_ME -> READ_ME2로 파일명 변경되며 testfolder 디렉터리 내부로 이동됨
```

## pwd

현재 경로 보여주기

## man

명령어 옵션 정보 보기

```bash
man man  # man 명령어 옵션 정보 보기 (보기 중 'q' 누르면 보기 모드에서 탈출)
man ls  # ls 명령어 옵션 정보 보기
```

## echo

대상 변수 내용 보기

```bash
echo $PATH  # PATH 환경 변수 설정상태 확인
```

## file

파일 유형을 분석하는 명령어. 인코딩, 개행 문자를 알려준다. 만약 파일에 ASCII 문자만 있으면 다른 에디터에서 인코딩을 지정해 저장했더라도 무조건 ASCII라고 표시된다.

```bash
$ file test.properties
test.properties: UTF-8 Unicode text, with very long lines, with CRLF line terminators
```

## iconv

파일의 인코딩을 변경한다. 이 명령은 만능이 아니라서, 인코딩간 호환 불가능한 유니코드가 있으면 실패한다.

```bash
$ iconv -f UTF-8 -t UTF-16 input.txt > output.txt # UTF-8 인코딩인 input.txt 파일을 UTF-16 인코딩의 output.txt 파일로 내보냄.
$ iconv -f UTF-8 -t UTF-16 input.txt -o output.txt # 위 명령과 같으나 OS에 따라 안될 수도...
$ iconv -t ASCII test.properties # test.properties를 ASCII 인코딩으로 변환하여 출력
$ iconv -c -t ASCII unicode.txt > ascii.txt # unicode.txt 파일을 ASCII 인코딩인 ascii.txt 파일로 내보냄. 인코딩 변환 중 오류가 발생하면 해당 문자는 건너뛴다.
```

## export

환경 변수 설정

```bash
export LANG = "ko.KR.UTF-8"
```

## printenv

현재 정의되어 있는 모든 변수 확인

## uname

```bash
# OS 커널 정보 조회
uname -a

# OS 정보 확인 #1
cat /etc/*release*

# OS 정보 확인 #2
cat /etc/issue

# 리눅스 배포 정보 확인
lsb_release -a
```

## netstat

네트워크 정보 표시. APT 패키지 이름은 `net-tools`임.

```bash
# ip 확인
netstat -in

# 열려있는 포트 + 프로세스 목록
netstat -tnlp
```

## ip

라우팅, 네트워크 디바이스, 인터페이스, 터널 객체들을 확인/조정하는 명령어.

```bash
# 라우팅 테이블 확인
ip route | grep default

# 할당된 모든 네트워크 인터페이스 확인
ip addr

# 'eth0'만 확인
ip addr show eth0
```

## nslookup

인터넷 네임 서버에 대화형으로 질의. DNS 서버에서 ip 받아오는 명령어임.

```bash
nslookup icanhazip.com

# one.one.one.one(클라우드 플레어) 서버에서 daum.net 검색
nslookup daum.net one.one.one.one
```

## dig

DNS 검색 유틸리티. `nslookup`이랑 비슷한데 뭐가 막 더 많이 나옴.

```bash
# 기본 네임서버에 icanhazip.com 레코드 질의
dig icanhazip.com

# dns.google.com 서버에 icanhazip.com 레코드 질의
dig example.com @dns.google.com

# -x는 역방향 검색 옵션
dig -x 172.217.25.78
```

## w

현재 로그인 중인 사용자 정보 표시

## diff

파일 비교

```bash
diff -r ./directory1 ./directory2  # 지정된 디렉터리들을 비교해서 한 쪽에만 존재하는 파일이름 출력
```

## awk

패턴 검색과 처리를 위한 언어.  
명령어의 이름은 개발자인 Alfred V. Aho, Peter J. Weinberger, Brian W. Kernighan 3인의 머리글자를 사용해서 만든 것이다.

```bash
awk '{ action}' filename

ls -l | awk '{print $0}'              # 전체 필드가 모두 나타나도록...
drwxr-xr-x   2   prof9i4  dba          512  4월   25일  15:44   a_dir
drwxr-xr-x   2   prof9i4  dba          512  4월   18일  23:53   b_dir

ls -l | awk '{print $1}'                    # 1번 필드만 나타도록...
ls -l | awk '{print $1, $9}'               # 1번과 9번 필드만 나타나도록...
ls -l | awk '{print $3 "\t" $4 "\t" $9}'                # Tab 키가 적용된 결과...
ls -lt | awk '{print $9, "is using", $5, "bytes"}'     # text 추가
ls -lt | awk '$5 <= 200 {print $0}'   # 5번 필드가 200 이하일 경우 출력  
ps -elf | awk '{print $2}' # ps -elf의 두 번째 필드인 PID만 출력된다.
```

## history

명령 이력 보기

```
history [-c] [-d offset] [n] or history -anrw [filename] or history -ps arg [arg...]
```

```bash
history  # 명령 이력 보기
!999  # 명령 이력의 999번 재입력
```

## zip/unzip

zip 파일 압축/압축해제.

```
zip [-options] [-b path] [-t mmddyyyy] [-n suffixes] [zipfile list] [-xi list]
unzip [-Z] [-opts[modifiers]] file[.zip] [list] [-x xlist] [-d exdir]
```

```bash
zip -r ../archive.zip ./*  # 현재 디렉터리의 모든 파일을 상위/archive.zip 파일로 압축
unzip archive.zip -d unarchive  # unarchive 디렉터리에 archive.zip 파일을 압축 해제
```

## tar

Tarball 혹은 TAR archive라고 하는 여러 파일과 디렉터리를 하나로 묶는 기능. tar 파일을 압축(archiving)하거나 압축해제한다.

```bash
# some-archive-1.0.0 디렉터리에 압축 해제
tar xvf some-archive-1.0.0.tgz
```

용량을 줄이는 진짜 압축(compress)은 gunzip(확장자 gz) 옵션인 `-z`를 적용해야 함.

```bash
# 현재 경로의 모든 파일을 xxx.tar.gz로 압축
tar cvfz example.tar.gz *

# 권한(permission)이 없는 파일 패스하며, 해당 경로의 모든 파일을 example.tar.gz로 압축
tar cvfz example.tar.gz * --ignore-failed-read

# 현재 디렉터리에 압축 해제
tar xvfz example.tar.gz

# test 디렉터리에 압축 해제
tar xvfz example.tar.gz -C test
```

#### options

- `x`: 묶음을 해제
- `c`: 파일을 묶음
- `v`: 묶음/해제 과정을 화면에 표시
- `f`: 파일 이름을 지정
- `p`: 권한(permission)을 원본과 동일하게 유지
- `C`: 압축을 풀 디렉터리를 지정할 때 쓰는 옵션
- `z`: gunzip을 사용.

## date

시스템 시간을 표시한다.

```bash
# Thu Dec 30 17:40:52 KST 2021
date

# 2021-12-30 17:40:49
date +'%Y-%m-%d %k:%M:%S'

# 콘솔에 출력
echo `date +%Y-%m-%d` `date +%H:%M:%S`

# 현재 시간을 파일명으로
touch `date +%Y-%m-%dT%H:%M:%S`
```

## make, make install

make는 대충 설명하면 소스 파일을 받아 직접 컴파일 후 설치하는 명령이다. 패키지 설치(apt)와 비교하면 이런 장점이 있다:

- 관리자 권한 없이 앱 설치 가능.
- 설치 경로를 직접 지정할 수 있다.
- 앱 삭제는 단순히 설치 경로를 폴더째로 지워버리면 된다.

[여기](https://jukki.tistory.com/4)에 잘 설명돼 있음.

보통 요딴식으로 진행한다:

```bash
# 파일 다운로드
wget https://dlcdn.apache.org//httpd/httpd-2.4.52.tar.gz

# 압축 풀기
tar xvfz httpd-2.4.52.tar.gz

cd httpd-2.4.52

# configure는 의존성을 확인하고 Makefile을 생성한다.
# 이 때 prefix 옵션으로 설치할 경로를 지정할 수 있다.
./configure --prefix=/home/noritersand/apache

# 컴파일
make

# 지정한 위치로 복붙
make install
```

`make install`은 컴파일된 바이너리 파일들을 지정한 위치로 재배치하는 최종 명령어다. 일부 MakeFile은 이 단계에서 재정리와 추가 컴파일을 하기도 한댄다.

> `make`: follows the instructions of the Makefile and converts source code into binary for the computer to read.  
> `make install`: installs the program by copying the binaries into the correct places as defined by ./configure and the Makefile. Some Makefiles do extra cleaning and compiling in this step.
> 출처: https://blogs.iu.edu/ncgas/2019/03/11/installing-software-makefiles-and-the-make-command/
