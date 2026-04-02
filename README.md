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
user.email=***ybseol315@gmail.com
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
