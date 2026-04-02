# BinniRepository

# Docker 및 터미널 기본 역량 강화 프로젝트

## 1. 프로젝트 개요
* **미션 목표**: 터미널 기본 조작 및 권한 관리 방법을 숙지하고, Docker 기본 운영 명령, 커스텀 이미지 빌드, 포트 매핑 및 볼륨 영속성을 검증하여 기초적인 컨테이너 인프라 활용 능력을 증명한다.

## 2. 실행 환경
* **OS**: Mac (macOS)
* **터미널**: zsh
* **Docker 버전**: Docker version 28.5.2
* **Git 버전**: git version 2.53.0

## 3. 수행 항목 체크리스트
- [x] 터미널 기본 조작 및 파일/디렉토리 관리
- [x] 권한 확인 및 변경 실습 (`chmod`)
- [x] Docker 환경 점검 (`docker info`, 버전 확인)
- [x] Dockerfile 생성 및 커스텀 이미지 빌드
- [x] 포트 매핑 및 호스트-컨테이너 연결 확인
- [x] Docker 볼륨 마운트 및 데이터 영속성 검증
- [x] Git 사용자 설정 및 GitHub 리포지토리 연동 완료

## 4. 검증 방법 요약표

| 검증 항목 | 사용한 명령어 / 확인 방법 | 결과 위치 |
|        ---        |                 ---                 |                 ---                 |
| **Git/GitHub 연동** | `git config --list`, GitHub 화면 캡처 | [5번 항목](#5-git-설정-및-github-연동) |
| **터미널/권한 실습** | `ls -al`, `mkdir`, `touch`, `chmod` 등 | [6번 항목](#6-터미널-조작-및-권한-실습) |
| **Docker 점검** | `docker info`, `docker images`, `docker ps` 등 | [7번 항목](#7-docker-설치-및-기본-점검) |
| **커스텀 빌드/포트** | `docker build`, `docker run -p`, 브라우저 접속 | [8번 항목](#8-커스텀-이미지-제작-및-포트-매핑) |
| **볼륨 영속성** | `docker volume create`, `docker run -v` | [9번 항목](#9-docker-볼륨-영속성-검증) |

---

## 5. Git 설정 및 GitHub 연동

### 5.1 Git 사용자 정보 설정
```bash
$ git config --list
credential.helper=osxkeychain
user.name=설영빈
user.email=ybs***@gmail.com
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
core.ignorecase=true
core.precomposeunicode=true
remote.origin.url=https://github.com/youngbin0315/BinniRepository.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.main.remote=origin
branch.main.merge=refs/heads/main
```

### 5.2 GitHub 연동 증거
![GitHub 연동 스크린샷](여기에 깃허브 리포지토리 화면을 캡처해서 드래그 앤 드롭 하세요)

---

## 6. 터미널 조작 및 권한 실습

### 6.1 터미널 조작 로그
```bash
# 위치 및 목록 확인
$ pwd
/Users/ybs***/BinniRepository

$ ls -al
total 8
drwxr-xr-x   4 student  student   128  4  2 17:28 .
drwxr-x---+ 22 student  student   704  4  2 18:04 ..
drwxr-xr-x  12 student  student   384  4  2 17:28 .git
-rw-r--r--@  1 student  student  2171  4  2 17:37 README.md

# 디렉토리 생성 및 이동, 빈 파일 생성
$mkdir test_dir
$ cd test_dir
$ touch test_file.txt

# 파일 복사, 이동/이름변경, 내용 확인
$cp test_file.txt copy_file.txt
$ mv copy_file.txt rename_file.txt
$ cat rename_file.txt
// 빈 파일이므로 아무 내용도 안 나오는 것이 정상입니다

# 삭제
$ rm rename_file.txt
$ cd ..
$ rm -rf test_dir
```

### 6.2 권한 변경 실습
```bash
# 실습 파일 및 디렉토리 생성
$ touch perm_file.txt
$ mkdir perm_dir

# 변경 전 권한 확인
$ ls -l
total 8
drwxr-xr-x  2 student  student    64  4  2 19:46 perm_dir
-rw-r--r--  1 student  student     0  4  2 20:05 perm_file.txt
-rw-r--r--@ 1 student  student  2171  4  2 17:37 README.md

# 권한 변경 (파일은 777, 디렉토리는 755)
$ chmod 777 perm_file.txt
$ chmod 755 perm_dir

# 변경 후 권한 확인
$ ls -l
total 8
drwxr-xr-x  2 student  student    64  4  2 19:46 perm_dir
-rwxrwxrwx  1 student  student     0  4  2 20:05 perm_file.txt
-rw-r--r--@ 1 student  student  2171  4  2 17:37 README.md
```

---

## 7. Docker 설치 및 기본 점검

### 7.1 데몬 동작 점검
```bash
$ docker info
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.29.1
    Path:     /Users/ybseol3157417/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.40.3
    Path:     /Users/ybseol3157417/.docker/cli-plugins/docker-compose

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 28.5.2
 Storage Driver: overlay2
  Backing Filesystem: btrfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
// 이하 생략
```

### 7.2 Docker 기본 운영 명령
```bash
# 이미지 다운로드 및 목록 확인
$ docker pull hello-world
$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED      SIZE
hello-world   latest    e2ac70e7319a   9 days ago   10.1kB
```
```bash
# 컨테이너 실행 및 중지, 목록 확인
$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

```bash
$ docker ps -a // ps: program status(프로그램의 상태) 실행 중인 것만 보여줌
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
c47e5dd982b6   hello-world   "/hello"   48 seconds ago   Exited (0) 47 seconds ago             epic_greider

# 로그 및 리소스 확인
$ docker logs c47e5dd982b6 // 본인의 hello-world 컨테이너 ID 또는 이름
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

```bash
$ docker stats --no-stream
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT   MEM %     NET I/O   BLOCK I/O   PIDS
```

## 7.3 ubuntu 컨테이너 진입 및 기초 명령
```bash
$ docker run -it ubuntu bash
root@컨테이너ID:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

root@컨테이너ID:/# echo "Hello Ubuntu"
Hello Ubuntu
root@컨테이너ID:/# exit
exit
```

## 7.4 컨테이너 종료/유지 (attach/exec) 차이점 관찰
docker attach: 이미 실행 중인 컨테이너의 메인 프로세스에 터미널을 직접 연결한다. 여기서 exit를 입력해 빠져나오면 컨테이너도 함께 완전히 종료된다.

docker exec: 실행 중인 컨테이너 내부에 새로운 쉘(프로세스)을 열어 접속한다. 여기서 exit로 빠져나와도 메인 프로세스는 계속 실행되므로 컨테이너가 종료되지 않고 유지된다.

(쉽게 해석한 버전)

attach: 컨테이너의 '메인 전원'에 직접 연결하는 방식. 여기서 빠져나오면(exit) 메인 전원도 같이 꺼지므로 컨테이너가 완전히 종료된다

exec: 컨테이너에 '보조 출입문'을 열고 들어가는 방식. 여기서 빠져나와도(exit) 메인 전원은 켜져 있으므로 컨테이너는 종료되지 않고 계속 실행된다

---

# 8. 커스텀 이미지 제작 및 포트 매핑

## 8.1 Dockerfile 제작

(A)웹 서버 베이스 이미지 활용(예: NGINX/Apache 등) + 정적 콘텐츠/설정만 교체

(B)Linux 베이스 이미지(예: ubuntu/alpine 등) + 기본 기능(패키지/사용자/환경변수/헬스체크 등) 추가

중 하나를 선택하여 기존 Dockerfile/이미지 기반의 커스텀 이미지를 만든다.

선택한 베이스 이미지: nginx:lastest (웹 서버)
커스텀 포인트 목적: NGINX의 기본 시작 페이지를 내가 만든 index.html 파일로 교체하여, 나만의 정적 웹 서버를 구동하기 위함.
```bash
# 작성한 Dockerfile 내용
FROM nginx:lastest
COPY index.html /usr/share/nginx/html/index.html



