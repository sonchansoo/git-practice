
### **Lab 6: Git 강의 노트**

#### **1. 버전 관리 시스템 (VCS)의 이해**

버전 관리 시스템은 소프트웨어 개발 및 문서 작업에서 파일의 변경 이력을 체계적으로 관리하고 여러 명의 사용자가 협업할 수 있도록 돕는 필수적인 도구입니다.

*   **버전 관리 방식**
    *   **변경점 저장 (Changes):** 초기 버전 이후의 변경된 내용(Delta)만 저장합니다.
    *   **스냅샷 저장 (Snapshots):** 각 버전마다 프로젝트 전체 파일의 상태를 그대로 사진 찍듯이 저장합니다. Git은 스냅샷 방식을 사용합니다.

*   **버전 관리 시스템 종류**
    *   **로컬 (Local):** 사용자 개인 컴퓨터에서만 버전을 관리합니다.
    *   **중앙 집중식 (Centralized):** 모든 버전 이력을 중앙 서버에서 관리하며, 개발자들은 이 서버에 접속하여 작업을 수행합니다.
    *   **분산 (Distributed):** 모든 개발자가 중앙 서버의 전체 이력을 자신의 컴퓨터에 복제하여 작업합니다. Git은 분산 버전 관리 시스템에 해당합니다.

#### **2. Git의 3가지 상태**

Git은 파일을 `Modified`, `Staged`, `Committed` 세 가지 상태로 관리하며, 각 상태는 특정 작업 공간과 연결됩니다.

1.  **Working Directory (작업 디렉토리):** 실제 파일들이 위치하는 프로젝트 폴더입니다. 파일이 수정되면 `Modified` 상태가 됩니다.
2.  **Staging Area (스테이징 영역):** 커밋(commit)할 변경 사항들을 선별하여 추가하는 가상의 공간입니다. `git add` 명령어로 파일을 추가하면 `Staged` 상태가 됩니다.
3.  **.git directory (Repository, 저장소):** 프로젝트의 모든 버전 이력(스냅샷)이 영구적으로 저장되는 곳입니다. `git commit` 명령어로 스테이징 영역의 파일들을 저장소에 기록하면 `Committed` 상태가 됩니다.

#### **3. Git 설치 및 최초 설정**

**설치 확인**
```bash
# 설치된 Git 버전 확인
$ git --version
```

**사용자 정보 설정 (최초 1회)**
커밋 기록에 남길 사용자 이름과 이메일 주소를 설정합니다. `--global` 옵션은 현재 컴퓨터의 모든 Git 저장소에 적용됩니다.
```bash
# 사용자 이름 설정
$ git config --global user.name "sonchansoo"

# 사용자 이메일 설정
$ git config --global user.email your-email-address@gachon.ac.kr
```

**기본 브랜치명 설정**
최근에는 `master` 대신 `main`을 기본 브랜치명으로 사용하는 추세입니다.
```bash
$ git config --global init.defaultBranch main
```

**설정 확인**
```bash
# 전체 설정 목록 확인
$ git config --list

# 설정 값이 어느 파일에 저장되었는지 함께 확인
$ git config --list --show-origin
```

#### **4. Git 저장소 생성 및 기본 명령어 흐름**

**저장소 초기화 (Repository Initialization)**
기존 프로젝트 폴더를 Git 저장소로 만들거나, 새로운 저장소를 생성합니다. 폴더 내에 `.git` 숨김 디렉토리가 생성됩니다.
```bash
$ git init
```

**상태 확인 (Checking Status)**
작업 디렉토리와 스테이징 영역의 현재 상태를 확인합니다. 어떤 파일이 변경되었는지, 스테이징되었는지 등을 보여줍니다.
```bash
$ git status
```

**스테이징 (Staging Files)**
커밋할 파일을 스테이징 영역으로 추가합니다. 이 과정을 '추적(track)'한다고도 합니다.
```bash
# 특정 파일 하나를 스테이징
$ git add README.md

# 여러 파일을 동시에 스테이징
$ git add main.py words.txt

# 현재 디렉토리의 모든 변경된 파일 및 새로운 파일을 스테이징
$ git add .
```

**스테이징 취소 (Unstaging a File)**
스테이징 영역에 추가된 파일을 다시 작업 디렉토리 상태로 되돌립니다.
```bash
$ git rm --cached [file_name]
```

**커밋 (Committing Changes)**
스테이징 영역에 있는 파일들을 하나의 버전(스냅샷)으로 저장소에 영구적으로 기록합니다. `-m` 옵션으로 커밋 메시지를 함께 기록해야 합니다.
```bash
$ git commit -m "initial commit"
```

**이력 확인 (Viewing Commit History)**
지금까지의 커밋 이력을 시간순으로 확인합니다.```bash
$ git log
```

#### **5. 특정 파일 무시하기 (`.gitignore`)**

로그 파일, 빌드 결과물 등 버전 관리할 필요가 없는 파일이나 디렉토리를 지정하면 Git이 해당 파일들을 추적하지 않습니다.

1.  프로젝트 최상위 디렉토리에 `.gitignore` 파일을 생성합니다.
2.  파일 내부에 무시하고 싶은 파일명이나 폴더명, 또는 패턴을 한 줄에 하나씩 작성합니다.

**`.gitignore` 작성 예시:**
```
# 모든 .a 확장자 파일 무시
*.a

# build 라는 이름의 모든 디렉토리 무시
build/

# doc/ 디렉토리 아래의 모든 .pdf 파일 무시
doc/**/*.pdf
```

#### **6. 브랜치 (Branch)**

**브랜치 이름 변경**
현재 브랜치의 이름을 변경합니다.
```bash
# 현재 브랜치 이름(master)을 main으로 변경
$ git branch -m master main
```
