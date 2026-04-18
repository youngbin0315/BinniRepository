# BinniRepository

# Docker 및 터미널 기본 역량 강화 프로젝트

## 1. 프로젝트 개요
* **미션 목표**: 터미널 기본 조작 및 권한 관리 방법을 숙지하고, Docker 기본 운영 명령, 커스텀 이미지 빌드, 포트 매핑 및 볼륨 영속성을 검증하여 기초적인 컨테이너 인프라 활용 능력을 증명한다.
### 1.1 프로젝트 디렉토리 구조 설계 기준
본 프로젝트는 코드의 유지보수성과 가독성을 높이기 위해 다음과 같은 기준으로 디렉토리를 구성했습니다.

- 최상위 폴더 (BinniRepository): Git 설정 파일(.gitignore)과 프로젝트 설명서(README.md) 등 전체 프로젝트를 설명하고 관리하는 메타 데이터 위주로 배치했습니다.

- 웹 서버 폴더 (my_web): 웹 애플리케이션 구동에 필요한 파일들을 격리했습니다.
  - Dockerfile: 도커 이미지 빌드를 위한 설정 파일.
  - src/ 폴더: 실제 서비스될 정적 소스 코드(index.html)만 따로 분리하여 관리합니다.

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

![capture_web](https://github.com/user-attachments/assets/08014026-5c36-4f70-a1c6-dd187982c61e)

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

### ※ 권한 숫자 표기(755, 644 등) 결정 규칙
리눅스의 파일 권한은 읽기(r=4), 쓰기(w=2), 실행(x=1)의 숫자를 더해서 표기합니다.

755 (디렉토리 적용): 소유자(4+2+1=7), 그룹(4+1=5), 기타 사용자(4+1=5). 디렉토리 접근 및 실행을 위해 주로 주어지는 권한입니다.

644 (일반 파일 적용): 소유자(4+2=6), 그룹(4=4), 기타 사용자(4=4). 누구나 읽을 수는 있지만, 수정은 소유자만 가능하도록 할 때 사용합니다.

실습에서는 테스트를 위해 perm_file.txt에 모든 권한(777)을 부여해 보았습니다.

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
 - docker attach: 이미 실행 중인 컨테이너의 메인 프로세스에 터미널을 직접 연결한다. 여기서 exit를 입력해 빠져나오면 컨테이너도 함께 완전히 종료된다.

 - docker exec: 실행 중인 컨테이너 내부에 새로운 쉘(프로세스)을 열어 접속한다. 여기서 exit로 빠져나와도 메인 프로세스는 계속 실행되므로 컨테이너가 종료되지 않고 유지된다.

(쉽게 해석한 버전)

 - attach: 컨테이너의 '메인 전원'에 직접 연결하는 방식. 여기서 빠져나오면(exit) 메인 전원도 같이 꺼지므로 컨테이너가 완전히 종료된다

 - exec: 컨테이너에 '보조 출입문'을 열고 들어가는 방식. 여기서 빠져나와도(exit) 메인 전원은 켜져 있으므로 컨테이너는 종료되지 않고 계속 실행된다

## 7.5 이미지와 컨테이너의 개념적 차이점

 - 빌드(Build) 관점: 이미지는 Dockerfile을 통해 만들어지는 '변하지 않는 템플릿'으로, 실행에 필요한 모든 환경과 소스 코드가 포장되어 있습니다.

 - 실행(Run) 관점: 컨테이너는 이 이미지를 바탕으로 메모리에 올라가 실제로 '살아 움직이는 실행 프로세스'입니다. 하나의 이미지로 여러 개의 독립적인 컨테이너를 실행할 수 있습니다.

 - 변경(Change) 관점: 작동 중인 컨테이너 내부에 접속해 파일을 임의로 변경하더라도, 원본 '이미지'는 변하지 않습니다. 영구적인 변경을 원한다면 Dockerfile이나 소스 코드를 수정하고 이미지를 다시 '빌드'해야 합니다.

---

## 8. 커스텀 이미지 제작 및 포트 매핑

### 8.1 Dockerfile 제작

(A)웹 서버 베이스 이미지 활용(예: NGINX/Apache 등) + 정적 콘텐츠/설정만 교체

(B)Linux 베이스 이미지(예: ubuntu/alpine 등) + 기본 기능(패키지/사용자/환경변수/헬스체크 등) 추가

중 하나를 선택하여 기존 Dockerfile/이미지 기반의 커스텀 이미지를 만든다.

선택한 베이스 이미지: nginx:lastest (웹 서버)
커스텀 포인트 목적: NGINX의 기본 시작 페이지를 내가 만든 index.html 파일로 교체하여, 나만의 정적 웹 서버를 구동하기 위함.
```bash
# 작성한 my_web/Dockerfile 내용
FROM nginx:lastest
COPY src/index.html /usr/share/nginx/html/index.html
```

※ 경로 선택 기준 및 환경 재현성

 - 상대 경로 (src/index.html): 내 컴퓨터(호스트)에서 소스 파일을 지정할 때 사용했습니다. 프로젝트를 다른 컴퓨터로 옮기더라도 폴더 내부 구조만 유지되면 잘 작동하기 때문입니다.

 - 절대 경로 (/usr/share/nginx/html/index.html): 도커 컨테이너 내부의 목적지를 지정할 때 사용했습니다. 컨테이너 내부는 항상 동일한 루트(/) 구조가 보장되기 때문입니다.

 - 재현성 확보: 위와 같이 COPY 명령어를 Dockerfile에 명문화함으로써, 누가 이 이미지를 빌드하든 항상 동일한 웹 페이지가 포함되도록 재현 가능한 환경을 구성했습니다.

### 8.2 빌드 및 실행 로그
```bash
# 빌드 명령
# my_web 폴더를 대상으로 빌드 명령 실행
$ docker build -t my-custom-nginx ./my_web
[+] Building 8.5s (7/7) FINISHED                                                                 docker:orbstack
 => [internal] load build definition from Dockerfile                                                        0.2s
 => => transferring dockerfile: 103B                                                                        0.0s
 => [internal] load metadata for docker.io/library/nginx:latest                                             2.8s
 => [internal] load .dockerignore                                                                           0.1s
 => => transferring context: 2B                                                                             0.0s
 => [internal] load build context                                                                           0.2s
 => => transferring context: 68B                                                                            0.0s
 => [1/2] FROM docker.io/library/nginx:latest@sha256:7150b3a39203cb5bee612ff4a9d18774f8c7caf6399d6e8985e9...
 => [2/2] COPY src/index.html /usr/share/nginx/html/index.html                                              0.4s
 => exporting to image                                                                                      0.2s
 => => exporting layers                                                                                     0.1s
 => => writing image sha256:70195e5bf5da7e1f527f99191b36211727806eaa835f34bbfce73b4303b88102                0.0s
 => => naming to docker.io/library/my-custom-nginx

# 포트 매핑하여 실행
$ docker run -d -p 8080:80 --name my_web my-custom-nginx
b059b22c3efeb383b95cdf8616901391581e08a3cceb68242da25485bc4eca1a
```

### 8.3 포트 매핑 접속 증거

![web_capture](https://github.com/user-attachments/assets/cdd3831b-4223-4c88-a9f9-02a8a95c7a73)

---

## 9. Docker 볼륨 영속성 검증

※ 데이터 증발 경험 및 볼륨 설정의 목적
실습 중 docker rm으로 컨테이너를 완전히 삭제하면, 컨테이너 내부에 임시로 만들어뒀던 파일이나 데이터가 모두 초기화(증발)되는 것을 확인했습니다. 이를 방지하기 위해 컨테이너의 특정 경로를 호스트의 공간과 연결하는 **도커 볼륨(Volume)**을 생성했습니다.

아래의 docker run -v my_data_volume:/data 명령어 설정을 통해, 컨테이너가 삭제되더라도 데이터는 호스트에 안전하게 남아 언제든 다시 연결할 수 있는 재현 가능한 영속성 환경을 구성했습니다.

### 9.1 볼륨 생성 및 확인
```bash
$ docker volume create my_data_volume
$ docker volume ls
DRIVER    VOLUME NAME
local     my_data_volume
```

### 9.2 컨테이너 연결 및 데이터 쓰기
```bash
$ docker run -d --name vol_test_container -v my_data_volume:/data nginx
// 실행 결과가 길기 때문에 생략
$ docker exec vol_test_container sh -c "echo 'Persistence Test Success' > /data/test.txt"
$ docker exec vol_test_container cat /data/test.txt     // 여기서 'Persistence Test Success'를 출력함
Persistence Test Success
```

### 9.3 컨테이너 삭제 후 데이터 유지 검증
```bash
$ docker stop vol_test_container
$ docker rm vol_test_container        // 'vol_test_container'를 종료 및 삭제

$ docker run -d --name vol_verify_container -v my_data_volume:/data nginx        // 'vol_verify_container'이라는 이름의 컨테이너로 이름을 바꾼 뒤 재 실행 준비

$ docker exec vol_verify_container cat /data/test.txt
Persistence Test Success     // 결과가 출력됨
```

### 9.4 검증 결과 요약 및 재현성

vol_test_container를 완전히 삭제했음에도 불구하고, 동일한 볼륨(my_data_volume)을 마운트하여 실행한 새로운 vol_verify_container에서 이전에 생성한 test.txt 파일이 그대로 조회되는 것을 확인하였습니다.
이를 통해 컨테이너가 삭제되더라도 데이터는 호스트에 안전하게 남아 언제든 다시 연결할 수 있는 재현 가능한 영속성 환경이 성공적으로 구성되었음을 검증했습니다.

---


## 10. 트러블 슈팅 및 미션 회고

### 10.1 첫 번째 트러블 슈팅: Git Push 충돌

### 미션 중 가장 어려웠던 지점 (문제 발생)
터미널과 Docker 실습을 모두 마치고 최종적으로 GitHub에 작업물을 올리려 할 때, git push origin main 명령어가 Updates were rejected (non-fast-forward) 에러와 함께 거절되는 문제가 발생했습니다.
원인을 분석해보니, 다음과 같은 두 가지 이유가 겹쳐 있었습니다.

1. GitHub 웹상에서 먼저 README.md를 수정하거나 캡처 이미지를 업로드한 내역이 로컬(내 PC)에 동기화되지 않은 상태에서 Push를 시도함.

2. Mac OS 환경에서 자동으로 생성되는 숨김 파일인 .DS_Store가 Git 추적 목록에 포함되어 불필요한 충돌을 일으킴.

### 해결 과정 및 근거

이 문제를 해결하기 위해 강제로 덮어씌우는(push -f) 대신, 안전하게 양쪽의 기록을 병합하고 원천적인 문제를 차단하는 방식을 택했습니다.

1. 원격 저장소 동기화 (Merge)
git pull origin main --allow-unrelated-histories 명령어를 사용하여 GitHub에 있는 최신 상태(캡처 이미지 등)를 내 컴퓨터로 안전하게 병합(Merge)하여 가져왔습니다.

2. .DS_Store 추적 제외 및 .gitignore 적용
최상위 경로에 .gitignore 파일을 생성하고 **/.DS_Store를 입력하여 앞으로 이 파일이 무시되도록 설정했습니다.

3. 캐시 삭제 및 재커밋
이미 Git이 추적하고 있는 기존 .DS_Store 파일들을 빼내기 위해 git rm -r --cached . 명령어로 캐시를 비운 뒤, git add .와 git commit -m "fix: .DS_Store 제외 및 gitignore 추가"를 진행했습니다.

### 깨달은 점

에러가 났을 때 무작정 명령어를 다시 치기보다, 에러 메시지(Updates were rejected)를 읽고 원격과 로컬의 '상태 차이'를 인지하는 것이 중요함을 배웠습니다. 또한, Docker의 폴더 구조 분리뿐만 아니라 Git을 통한 버전 관리에서도 .gitignore를 활용해 내가 딱 필요한 소스코드만 관리하는 것이 인프라 관리의 기본임을 깨달았습니다.

### 10.2 두 번째 트러블 슈팅: 포트 충돌 및 컨테이너 이름 중복 문제

어려웠던 지점 (문제 발생)
커스텀 Nginx 이미지를 빌드한 후, 테스트를 위해 docker run -d -p 8080:80 --name my_web my-custom-nginx 명령어를 실행했을 때 다음과 같은 에러 메시지가 발생하며 컨테이너가 실행되지 않았습니다.

Error response from daemon: Conflict. The container name "/my_web" is already in use by container...

혹은 Bind for 0.0.0.0:8080 failed: port is already allocated.

해결 과정 및 근거
이 에러는 이전에 테스트로 실행했던 동일한 이름(my_web)의 컨테이너가 삭제되지 않고 남아있거나, 8080 포트를 이미 다른 백그라운드 프로세스(혹은 멈춰있는 컨테이너)가 점유하고 있기 때문에 발생한 문제였습니다.

원인 프로세스 파악: docker ps -a 명령어를 입력하여 현재 시스템에 남아있는 모든 컨테이너 목록을 조회했습니다. 그 결과, 이전에 테스트하다가 Exited 상태로 멈춰있는 my_web 컨테이너를 발견했습니다.

기존 리소스 정리: 컨테이너가 멈춰있더라도 이름과 포트 매핑 정보는 그대로 유지된다는 것을 확인하고, docker stop my_web (실행 중인 경우) 명령어로 확실히 종료한 뒤, docker rm my_web 명령어로 기존 컨테이너를 완전히 삭제하여 점유된 자원(이름과 포트)을 해제했습니다.

재실행 및 성공: 자원 정리가 완료된 후 다시 docker run -d -p 8080:80 --name my_web my-custom-nginx 명령어를 입력하니 정상적으로 컨테이너 아이디가 출력되며 웹페이지 접속에 성공했습니다.

깨달은 점
도커에서 터미널 창을 닫거나 컨테이너가 에러로 멈췄다고 해서, 그 컨테이너가 완전히 삭제된 것은 아니라는 '컨테이너의 생명주기(Lifecycle)' 개념을 명확히 이해하게 되었습니다. 새로운 컨테이너를 띄우기 전에는 항상 기존에 사용하려던 자원(이름, 포트)이 확실히 비워져 있는지 docker ps -a 등으로 점검하는 습관이 중요함을 배웠습니다.

---
