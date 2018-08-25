---
layout: post
date: 1970-01-01 00:00:00 +0900
title: 'Unix/Linux: 쉘 명령어'
categories:
  - os
  - unix/linux
tags:
  - todo
  - unix
  - linux
---

#### 참고한 글
- [https://wiki.kldp.org/HOWTO/html/Adv-Bash-Scr-HOWTO/](https://wiki.kldp.org/HOWTO/html/Adv-Bash-Scr-HOWTO/)

#### 테스트 환경
- CentOS Linux release 7.1.1503 (Core)

## grep
패턴 검색
```
grep [OPTIONS] PATTERN [FILE...]
grep [OPTIONS] [-e PATTERN | -f FILE] [FILE...]
```

**options**:
- `-r`: 지정된 대상이 폴더일 경우 하위의 파일을 검색
- `-c`: 패턴이 일치하는 행의 수를 출력
- `-i`: 비교시 대소문자를 구별 안함
- `-v`: 지정한 패턴과 일치하지 않는 행만 출력
- `-n`: 행의 번호를 함께 출력
- `-l`: 패턴이 포함된 파일의 이름을 출력
- `-w`: 패턴이 전체 단어와 일치하는 행만 출력

옵션을 생략할 경우 기본동작은 현재 경로 대상이며 패턴이 검색된 파일명과 해당 행의 문자열 출력한다.
```bash
grep -n 'http' httpd.conf # httpd.conf 파일에서 'httpd' 패턴을 찾아 행번호와 문자열 출력
grep -l -r 'error' /userhome/* # /userhome 아래의 모든 경로에서 'error' 패턴을 검색해 파일명만 출력
grep -l -v 'error' * # 모든 파일 중 'error' 패턴이 검색되지 않는 파일명만 출력
```

### 출력 필터링
```bash
cat ./httpd.conf | grep root # httpd.conf 파일 중 root가 포함된 행만 출력
tail -f access_log | grep error # access_log 파일의 변화를 추적하되 'error'가 포함된 행만 출력
```

## ln
바로가기를 생성한다.
```
ln target
```

**options**:
- `-s`: 하드 링크 대신 심볼릭 링크를 생성한다.

```bash
ln -s ~/was/upload ./upload
# 현재 경로에 upload라는 심볼릭 링크를 생성한다. 이 링크의 실제 물리경로는 home/was/upload
```

## curl
지정한 경로로 HTTP 요청을 전송한다.
```bash
curl http://tistory.com
```

## wget
콘텐츠 다운로드 명령어. 지정된 URL의 결과를 파일로 다운로드한다.

```bash
# example.com의 "index.html" 제목 페이지를 파일로 내려받기
wget http://www.example.com/

# GNU FTP 사이트로부터 Wget의 소스 코드 내려받기.
wget ftp://ftp.gnu.org/pub/gnu/wget/wget-latest.tar.gz
```

## find
파일찾기
```bash
find ./ -name 'log' # 이름이 'log'인 모든 파일과 디렉토리를 현재 경로와 하위에서 검색
find ./ -name '*.log' -type f # 확장자가 'log'인 모든 파일만 현재 경로와 하위에서 검색
```

## rm/rmdir
파일 혹은 디렉토리 삭제
```
rm 대상파일경로
rmdir 대상디렉토리경로
```
```bash
rm -rf ./target # target 디렉토리 내의 모든 파일과 하위 디렉토리를 프롬프트 없이 삭제
rm -f ./some-link # 심볼릭 링크 삭제
rmdir ./test-folder # 비어있는 디렉토리 삭제
```

## ps
실행중인 프로세스 확인
```bash
ps -ef
ps -eF
ps -eLf
ps -eLF
```

## kill
실행중인 프로세스를 죽인다(?).
```
kill 프로세스아이디
```

## pkill
프로세스를 이름으로 죽인다(??).
```bash
pkill -9 -ef catalina
```

**option**:
- `-9`: kill을 의미
- `-e`: 로그 출력
- `-f`: 명령행 전체 참조 옵션

## cd
명령행의 경로 이동
```bash
cd /  # 루트(최상위 기본) 경로로 이동
cd ~  # 로그인 유저의 홈으로 이동
cd ..  # 한단계 상위 경로로 이동
cd /bin/lib  # bin 하위의 lib으로 이동
```

## ls
해당 디렉토리 내부 리스트 보기
```bash
ls  # 내부 리스트 보기
ls -a  # 내부 리스트 모두 보기 (숨김/설정 파일까지)
```

## vi
편집기 실행
```bash
vi test.txt  # 해당경로에 test.txt 파일 편집모드 (파일 없을경우 생성하며 편집모드)
```

## cat/more
파일 내용 출력
```bash
cat Test.txt  # Test.txt 파일 내용을 화면에 출력
more Test.txt  # Test.txt 파일 내용을 화면단위로 분할하여 출력
less  # more를 보완한 명령어 ( less 옵션 파일명 )
```

## cp
파일 복사
```bash
cp test.txt testfolder  # (내부경로) test.txt 파일을 testfolder 디렉토리 내부에 복사
cp -a ... ...  # 파일의 소유권과 각종 정보 유지하여 복사 (위의 경우는 유지안됨)
```
* 파일 복사나 이동은 대상 경로의 존재여부에 따라 결과가 달라진다. [이에 대해 잘 정리된 문서 링크](https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4%EC%97%90%EC%84%9C_%ED%8F%B4%EB%8D%94_%ED%86%B5%EC%A7%B8%EB%A1%9C_%EB%B3%B5%EC%82%AC%ED%95%98%EA%B8%B0).

## mkdir
디렉토리(폴더) 생성
```bash
mkdir test  # 해당경로에 test 디렉토리 생성
```

## mv
디렉토리와 파일명 변경(이동)한다.
```bash
mv test test2  # test -> test2 디렉토리명 변경
mv test.txt test2.doc  # test.txt -> test.doc 파일명 변경
mv test.txt ./testfolder/  # test.txt 파일 testfolder 디렉토리내로 이동
mv test.txt ./testfolder/test2.txt  # test.txt -> test2.txt 파일명 변경되며 testfolder 디렉토리 내부로 이동됨
```

## pwd
현재 경로 보여주기

## man
명령어 옵션 정보 보기
```bash
man man  # man 명령어 옵션 정보 보기 (보기 중 'q' 누르면 보기 모드에서 탈출)
man ls  # ls 명령어 옵션 정보 보기
```

## | (파이프)
둘이상의 명령어를 연결

## > , < , >> (리다이렉션)
```bash
명령어>파일  # 파일이 없다면 생성하고 있다면 기존내용을 지움
명령어>>파일  # 파일이 없다면 생성하고 있다면 기존내용을 추가
명령어<파일  # 파일의 표준입력을 받는다
```

## echo
대상 변수 내용 보기
```bash
echo $PATH  # PATH 환경변수 설정상태 확인
```

## export
변수 값 설정
```bash
export lang = ko.KR.UTF-8  # 변수에 값 담고
export lang  # 영구 저장 설정
```

## printenv
현재 정의되어 있는 모든 변수 확인

## uname
```bash
uname -a  # Mac OS bit 확인.
```

## system_profiler
시스템 정보 확인

## top
``` bash
top -o cpu  # cpu 자원을 가장 많이 점유하고 있는 프로세스순 보기
```

## netstat
```bash
netstat -in  # ip 확인
netstat -tnlp  # 열려있는 포트 + 프로세스 목록
```

## ps
해당 디바이스 프로세스 리스트 보여주기

## w
현재 로그인 중인 사용자 정보 표시

## diff
파일 비교
```bash
diff -r ./directory1 ./directory2  # 지정된 폴더들을 비교해서 한 쪽에만 존재하는 파일이름 출력
```

## df
```bash
df -h  # 현재 시스템 파티션별 하드디스크 용량을 %및 Byte로 표시
```

**options**:
- `-h`: 크기를 KB, MB, GM 으로 자동 전환하여 표시

## awk
패턴 검색과 처리를 위한 언어.
명령어의 이름은 개발자인 Alfred V. Aho, Peter J. Weinberger, Brian W. Kernighan 3인의 머리글자를 사용해서 만든 것이다.
```bash
awk '{ action}' filename

OS/tdir] ls -l | awk '{print $0}'              # 전체 필드가 모두 나타나도록...
drwxr-xr-x   2   prof9i4  dba          512  4월   25일  15:44   a_dir
drwxr-xr-x   2   prof9i4  dba          512  4월   18일  23:53   b_dir

OS/tdir] ls -l | awk '{print $1}'                    # 1번 필드만 나타도록...
OS/tdir] ls -l | awk '{print $1, $9}'               # 1번과 9번 필드만 나타나도록...
OS/tdir] ls -l | awk '{print $3 "\t" $4 "\t" $9}'                # Tab 키가 적용된 결과...
OS/tdir] ls -lt | awk '{print $9, "is using", $5, "bytes"}'     # text 추가
OS/tdir] ls -lt | awk '$5 <= 200 {print $0}'   # 5번 필드가 200 이하일 경우 출력  
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
압축/압축해제
```
zip [-options] [-b path] [-t mmddyyyy] [-n suffixes] [zipfile list] [-xi list]
unzip [-Z] [-opts[modifiers]] file[.zip] [list] [-x xlist] [-d exdir]
```
```bash
zip -r ../archive.zip ./*  # 현재 디렉토리의 모든 파일을 상위/archive.zip 파일로 압축
unzip archive.zip -d unarchive  # unarchive 디렉토리에 archive.zip 파일을 압축 해제
```

## 기타
자주 쓰이는 환경변수 PATH 추가하기
예를 들어 주로 찾게되는 경로에 /usr/local/progdir 를 넣고 싶으면 아래와 같이 하면 된답니다.

PATH=$PATH:/usr/local/progdir export PATH

위의 명령은 기존에 있는 PATH 변수의 뒤에다가 좀전의 /usr/local/progdir 이 경로를 달아주기만 하는거랍니다.

그리고 export 명령으로 이 변수가 쉘환경에 계속 기본적으로 사용되게끔 해주는거라고 하네요..

기타 명령어 & Tip
터미널을 열고 ls를 입력하면 디렉토리와 파일이 나오죠. 근데 어느 것이 디렉토리고 어느것이 파일인지는

ls -l 로 봐야하는데 손이 잘 안가고 한 화면에 많은 양을 표시할 수가 없죠.

그래서 ls -F 명령어를 쓰시면 되는데요, 역시 마찬가지로 매번 -F를 붙이는 게 귀찮죠.

따라서, 터미널을 열고 아래와 같이 입력합니다.

$ sudo pico /etc/bashrc

그런 다음 루트 패스워드를 넣고, 에디터가 뜨면 맨 아래줄에 다음과 같이 추가해줍니다.

alias ls = 'ls -F'

다음, Ctrl + X 한 다음 y를 입력하면 쉘로 빠져나오게 됩니다.

다음 터미널을 종료시키면 이제부터는 ls만 쳐도 ls -F가 되어있는 상태가 됩니다.



ls를 했는데 파일들이 엄청 많아서, 이 중에서 원하는 것만 추리고 싶을 때 사용할 수 있는 방법입니다.

$ ls | grep 원하는글자

grep 이라는 명령어는 텍스트를 추려내는 기능을 갖고 있습니다.

따라서 grep의 파이프는 ls 말고도 cat이나 man 등의 명령어에도 이용할 수 있답니다. 예) man ls | grep file

원하는 글자가 아닌, 빠르게 스크롤되는 화면을 천천히 보고싶을 때는 파이프 뒤에 more를 붙이시면 됩니다. 예) ls | more



say  # 다음 입력되는 영어 문장/단어를 읽어줌...

EX) $ say Eclipse Start



스크린샷 이미지 파일의 기본 저장경로 변경하기

$ defaults write com.apple.screencapture location /Users/seokkoh/Desktop



현 디렉토리의 파일들중 파일명에 -hd 가 포함된 파일들 전부 삭제하기

$ rm -f *-hd.*

현 디렉토리의 파일 개수 확인하기 :

$ ls | wc -l
