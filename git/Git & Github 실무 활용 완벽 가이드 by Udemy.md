# 섹션1: 강의 오리인테이션

## 수강을 환영합니다

- “New developer? The best time to learn Git is yesterday.”
- “The best time to plant fruit trees is five years ago…”
- →  “The second best time is now”

# 섹션2: Git을 소개합니다!

## Git이란 정확히 무엇인가?

### VCS

- VCS → Version Control System

### Version Control

- Version control is software that **tracks and manages changes** to files over time.
- Version control systems generally allow users to revisit earlier versions of the files, compare changes between versions, undo changes, and a whole lot more.

## Git의 역사에 대한 개요

### Linus Torvalds

- Linux 팀에서 다른 버전 관리 시스템인 BitKeeper를 사용하고 있었음
- 어떤 사유로 인해 리눅스 팀이 더 이상 무료로 사용할 수 없게 됨
- 그래서 이것이 Mercurial을 포함해 몇 가지 버전 관리 시스템이 생겨나게 한 원동력이 됨
- Linus가 이 부분에 굉장히 큰 불만을 드러냄
- 유료 버전에서는 그가 원하는 기능과 속도가 충족되었으나 무료 버전에는 없었음 → 그가 원한 것은 빠른 속도를 갖춘 다양한 기능의 오픈소스
- **“그러면 내가 만들지 뭐”** → 2005년 4월 작업에 열중

### Behind The Name

[Git](https://en.wikipedia.org/wiki/Git)

- Torvalds referred to Git as “the stupid content tracker” while he was working on it. Eventually he settled on the name Git.
- The official Git source code explanes a couple different meanings for the name, depending on your mood:
    - a random three-letter, conbination that is pronounceable, and not actually used by any common UNIX command.
    - stupid. contemptible and despicable. simple.
    - “global information tracker”: you’re in a good mood, and it actually works for you. Angels sing, and a light suddenly filles the room.
    - “g#ddamn idiotic truckload of sh*t”: when it breaks
    - 영국 속어로 ‘반갑지 않은 손님’

## Git vs. Github: 차이점은 무엇일까?

### Git ≠ Github

- Git is the version control software that runs locally on your machine. You don’t need to register for an account. You don’t need the internet to use it. You can use Git without ever touching Github.
- Github is a service that hosts Git repositories in the cloud and makes it easier to collaborate with other people. You do need to sign up for an account to use Github. It’s an online place to share work that is done using Git.

# 섹션3: 설치 및 설정
## 터미널 vs. GUI

### Git Is (Primarily) A Terminal Tool

- Git was created as command-line tool. To use it, we run various git commands in a Unix shell. This is not the most user friendly experience, but it’s at the very core of Git!

### The Rise of GUI’s

- Over the last few years, companies have created graphical user interfaces for Git that allow people to use Git without having to be a command-line expert.
- Popular Git GUI’s include:
    - Github Desktop
    - SourceTree
    - Tower
    - GitKraken
    - Ungit
    
    [Git - GUI Clients](https://git-scm.com/downloads/guis)
    

### There is a lot of stupid GUI gate-keeping

## Windows에서 Git 설치하기

- Git은 유닉스 기반의 인터페이스로 실행되길 원하고 bash라는 shell이 리눅스 컴퓨터와 맥의 기본 쉘이기 때문에 윈도우에서는 command prompt가 있지만 유닉스 기반이 아님
- 그래서 윈도우에는 Git bash를 통해 사용함 → 윈도우에서 유닉스가 하는 bash 경험들을 모방할 수 있게 해주고 Git도 함께 제공

## Git 이름 및 이메일 구성하기

### Configuring Git

- To configure the name that Git will associate with your work, run this command:
    
    ```
    git config --global user.name "gonasooc"
    ```
    
- Do the same thing for your email using the following command. When we get to Github, you’ll want your Git email address to match your Github account.
    
    ```
    git config --global user.email doupler@gmail.com
    ```
    

## 터미널 집중 학습: 소개

### 명령어 집중 학습

## 터미널 집중 학습: 탐색

### 명령어

- List → Use `ls` to list the contents of your current directory. → list의 축약어, 현재 디렉토리 또는 폴더에 있는 컨텐츠를 나열
- `start .` (맥은 `open .`) → 윈도우탐색기
- `ls 폴더명` → 해당 폴더 내 list 확인
- `clear` → 터미널 클리어
- `ls 상위폴더/하위폴더` → 바로 하위폴더로 접근할 수 없음, 해당 상위폴더와 함께 작성해서 list 출력
- `pwd` → Print Working Directory(현재 경로 출력): Prints the path to the working directory (where you currently are)
- Change Directory → Use `cd` to change and move between folders ex) `cd 폴더명`
- `cd ..` → Use `cd ..` to “back up” one directory

## 터미널 집중 학습: 파일 및 폴더 생성
- `touch purple.txt` → 새 파일 만들기
- `touch test.js test.py` → 한 번에 여러 파일 생성 가능
- `touch folder/test.txt` → 하위 폴더명을 통해 해당 폴더 아래에 파일 생성 가능
- `mkdir Test` → Test라는 폴더를 만듬, 폴더 생성 명령어
- `mkdir SeaTurtles LandTurtles` → 파일과 동일하게 여러 개의 폴더 한번에 만들 수 있음

## 터미널 집중 학습: 파일 및 폴더 삭제

- `rm test.txt` → test.txt라는 파일을 삭제(휴지통이 아니라 바로 삭제)
- `rm test.txt delete.xls` → 동일하게 여러 개의 파일을 삭제할 수 있음
- `rm -rf Test` → Test 폴더를 삭제, 플래그를 통해서 옵션을 변경할 수 있음
- `ls -a` → ls 또한 플래그를 통해 숨겨진 파일을 다 볼 수 있음

# 섹션4: Git의 기초: 추가하기와 커밋하기

## Git Repo란 무엇인가?

### Repository

- **A Git “Repo” is a workspace which tracks and manages files within a folder.**
- Anytime we want to use Git with a project, app, etc we need to create a new git repository. We can have as many repos on our machine as needed, all with separate histories and contents
- 캡슐화된 거품처럼 각 저장소는 각각 연결되지 않고 독립된 이력과 컨텐츠를 갖고 있음

## 우리의 첫 번째 명령어: git init과 git status

### Our First Git Command!

- `git status` give information on the current status of a git repository and its contents
- It’s very useful, but at the moment we don’t actually have any repos to check the status of!
- Use `git init` to create a new git repository. Before we can do anything git-related, we must initialize a repo first!
- This is something you do once per project. Initialize the repo in the top-level folder containing your project → 터미널에서 어느 디렉토리에 있든 새 저장소를 초기화합니다

## Git 초심자가 흔히 저지르는 실수

- git은 최상위 폴더에서 탑다운으로 모든 디렉토리를 관리(Git Tracks A Directory and All Nested Subdirectories)
- 어떤 프로젝트를 위해 이 디렉토리에서 변경하는 파일이나 폴더는 git에 의해 추적됨
- git으로 트래킹되는 폴더 안에서 또 다시 git init을 하는 건 git 입장에서 혼란스러울 수 있음

## commit 위크플로우 개요

### Committing

- The most important Git feature!
- **Working Directory(`git add`) → Staging Area(`git commit`) → Repository**

## git add로 변경사항 스테이징하기

### Adding

- Use git add to add specific files to the staging area. Separate files with spaces to add multiple at once.

## 이제 드디어 git 명령어를 배울 시간

### Git Commit

- **We use the `git commit` command to actually commit changes from the staging area.**
- When making a commit, we need to provide a commit message that summarizes the changes and work snapshotted in the commit
- `git commit -m ‘git message’` → The -m flag allows us to pass in an inline commit message, rather than launching a text editor. We’ll learn more about writing good commit messages later on.

## git log 명령어(그리고 더 많은 커밋)

`git add .` → 한번에 모든 변경사항을 스테이지에 넣음

# 섹션5: 커밋과 관련 주제 자세히 알아보기

## git 문서 탐색

### Reference

https://git-scm.com/docs

### Book

https://git-scm.com/book/en/v2

## 커밋을 원자적으로 유지하기

### Atomic Commits

- When possible, a commit should encompass a single feature, change, or fix. In other words, try to **keep each commit focused on a single thing. → 각각의 커밋은 한 가지에 집중하도록 하라는 것(기능과 상황에 맞게 분류해서 commit)**
- This makes it much easier to undo or rollback changes later on. It also makes your code or project easier to review.

## 커밋 메시지: 현재 또는 과거 시제?

### Present-Tense Imperative Style??

- From the Git docs:
    
    ```
    Describe your changes in imperative mood, e.g. "make xyzzy do frotz" instead of "[This patch] makes xyzzy do frotz" or "I changed xyzzy to do frotz", as if you are giving orders to the codebase to change its behavior.
    ```
    
    - 현재 시제에 명령문 권장
- Which tense should be used on a Git commit message?
    
    https://medium.com/@corrodedlotus/which-tense-should-be-used-on-a-git-commit-message-121cb641134b
    

## VIM 빠져나오기 및 Git의 기본 편집기 구성하기