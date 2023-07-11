# 섹션1: 강의 오리인테이션

## 수강을 환영합니다

- “New developer? The best time to learn Git is yesterday.”
- “The best time to plant fruit trees is five years ago…”
- → “The second best time is now”

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
  - “g#ddamn idiotic truckload of sh\*t”: when it breaks
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

### VIM

- 터미널 내장 에디터
- VIM을 학습하기보다는 commit message 없이 git commit을 입력했을 때 vim으로 접근하게 됨
- `i` → insert 모드에서 commit message를 입력, `esc`로 insert mode를 빠져 나와서 `:wq` (write quit) 로 빠져나올 수 있음

## Git Log 명령어 자세히 알아보기

- 많은 기능들이 있지만 주로 사용하게 되는 건 `git log —oneline`
- 한 줄로 log를 보게 되기 때문에 암묵적으로 첫 줄은 해당 commit을 요약하고 그 이후에 상세 내용을 적는 것
  https://git-scm.com/docs/git-log

## Amend로 실수 수정하기

### Amending Commits

- Suppose you just made a commit and then realized you forgot to include a file! Or, maybe you made a typo in the commit message that you want to correct.
- Rather than making a brand new separate commit, you can “redo” the previous commit using the —amend option
- **바로 직전에 커밋한 것을 편집하거나 실행 취소 또는 업데이트를 할 수 있게 해줌, 바로 직전 커밋만 적용 가능**
- ex) 1.txt, 2.txt 먼저 커밋하고 3.txt를 누락했다면 → `git add 3.txt` → `git commit —amend` → vim insert mode에서 commit message 수정 후 `:wq`

## .gitignore로 파일 무시하기

### Ignoring Files

- We can tell Git which files and directories to ignore in a given repository, using a **.gitignore** file. This is useful for files you know you NEVER want to commit, including:
  - Secrets, API keys, credentials, etc.
  - Operating Systems files (.DS_Store on Mac)
  - Log files
  - Dependencies & packages
- Create a file called .gitignore in the root of a repository. Inside the file, we can write patterns to tell Git which files & folders to ignore:
  - `.DS_Store` will ignore files named .DS_Store
  - `folderName/` will ignore an entire directory
  - `*.log` will ignore any files with the .log extension
  - 슬래시가 없으면 파일 / 슬래시가 끝에 있으면 디렉토리
- .gitignore 체크할 만한 사이트
  https://www.toptal.com/developers/gitignore

# 섹션6: 브랜치(branch)로 작업하기

## 마스터 브랜치(또는 메인 브랜치?)

### Master? Main?

- In 2020, Github renamed the default branch from **master** to **main**. The default Git branch name is still **master**, though the Git team is expoloring a potential change. **We will circle back to this shortly.**

## HEAD가 대체 뭘까?

### HEAD

- We’ll often come across the term **HEAD** in Git.
- HEAD is simply a pointer that refers to the current “location” in your repository. It points to a particular branch reference.
- So far, HEAD always points to the latest commit you made on the master branch, but soon we’ll see that we can move around and HEAD will change!

## Git Branch로 모든 브랜치 보기

### Viewing Branches

- Use **git branch** to view your existing branches. The default branch in every git repo is master, though you can sconfigure this.
- Look for the \* which indicates the branch you are currenly on.

## 브랜치 생성 및 전환하기

### Creating Branches

- Use **git branch <branch-name>** to make a new branch based upon the current HEAD
- This just creates the branch. It does not switch you to that branch (the HEAD stays the same)

### Switching Branches

- Once you have created a new branch, use **git switch <branch-name>** to switch to it.
- `checkout`과 비슷한 명령어

## 기타 옵션: Git Checkout 대 Git Switch

### Another way of switching??

- Historically, we used git **checkout <branch-name>** to switch branches. This still works.
- The **checkout** command does a million additional things, so the decision was made to add a standalone switch command which is much simpler.
- You will see older tutorials and docs using checkout rather than switch. Both now work.

### Creating & Switching

- Use **git switch** with the **-c** flag to create a new branch AND switch to it all in one go.
- Remember -c as short for “create”
- #git checkout -b branch-name 도 동일하게 작동

## 브랜치 삭제 및 이름 바꾸기

- `git branch -d 삭제할브랜치` → 브랜치 삭제
- `git branch -D 삭제할브랜치` → 만약에 merge가 안 된 상태라면 merge가 되지 않았다는 에러 메세지와 함께 삭제가 안 되는데, 대문자 D로 강제 옵션을 줄 수 있음
- `git branch -m 새로운브랜치명` → 해당 브랜치를 새로운브랜치명으로 변경
- `git branch -m 기존브랜치명 새로운브랜치명` → 다른 브랜치에 HEAD 라면 기존브랜치명도 명시해줘야 함
  https://mylko72.gitbooks.io/git/content/branch/checkout.html

# 섹션7: 브랜치 병합하기, 맙소사!

## 병합 명령어 소개

### Merging

- Branching makes it super easy to work within self-contained contexts, but often we want to incorparate changes from one branch into another!
- We can do this using the **git merge** command
- The merge command can sometimes confuse students easly on. Remember these two merging concepts:
  - We merge branches, not specific commits → 특정 커밋이 아니라 브랜치를 병합
  - We always merge to the current HEAD branch → 항상 현재 HEAD 브랜치에 병합

## 빨리 감기 병합 수행하기

- 커밋을 따라잡기 위해 병합의 기준이 되는 브랜치를 빨리 감기하는 병합
- 깃의 측면에서 보면 아주 쉬운 병합 → 포인터를 특정 숫자의 커밋으로 옮기면 되고 아니면 특정 커밋까지 빨리 감기

## 병합 시각화하기

### Merging Made Easy

- **To merge, follow these basic steps:**
  1. Switch to or checkout the branch you want to merge the changes into (the receiving branch)
  2. Use the **git merge** command to merge changes from a specific branch into the current branch.

## 병합 커밋 생성하기

- 병합 커밋은 두 개의 다른 부모 커밋을 갖는다는 것
- ex) Merge branch ‘new-branch’

## 이런, 병합 중에 충돌이 발생했다!

### Heads Up!

- Depending on the specific changes your are trying to merge, Git may not be able to automatically merge. This results in **merge conflicts**, which you need to manually resolve.

### Conflict Markers

- The content from your current **HEAD** (the branch you are trying to merge content into) is displayed **between the <<<<<<< HEAD and =======**

### Resolving Conflicts

- Whenever you encounter merge conflict, follow these steps to resolve them:
  1. Open up the file(s) with merge conflicts
  2. Edit the file(s) to remove the conflicts. Decide which branch’s content you want to keep in each conflict. Or keep the content from both.
  3. Remove the conflict “markers” in the document
  4. Add your changes and then make a commit!

## 병합 충돌 해결하기

- 병합 커밋과 다르지는 않음 → 다만 커밋하기 위해 편집 작업이 필요할 뿐

## VSCode를 사용하여 충돌 해결하기

- Accept Current Change / Accept Incoming Change / Accept Both
- 직접 충돌 지점을 수정해서 별도의 스테이징/커밋을 작성해도 괜찮음

# 섹션8: Git Diff로 변경사항 비교하기

## Git Diff 명령어 소개

### Git Diff

- We can use the **git diff** command to view changes between commits, branches, files, our working directory, and more!
- We often use git diff alongside commands like git status and git log, to get a better picture of a repository and how it has changed over time.

## Diff 읽는 방법 1

### Compared Files

- For each comparison, git Explains which files it is comparing. Usually this is two versions of the same file.
- Git also declares one file as “A” and the other as “B”.

### File Metadata

- You really do not need to care about the file metadata.
- The first two numbers are the hashes of the two files being compared. The last number is an internal file mode identifier.

### Markers

- File A and File B are each assigned a symbol.
  - File A gets a minus sign (-)
  - File A gets a plus sign (+)

### Chunks

- A diff won’t show the entire contents of a file, but instead only shows portions or “chunks” that were modified.
- A chunk also includes some unchanged lines before and after a change to provide some context

## 작업 디렉토리 변경사항 보기

### git diff

- git diff HEAD lists all changes in the working tree since your last commit.
- git diff는 워킹 디렉토리와 스테이지 영역의 차이점을 보여주고, git diff HEAD는 HEAD가 가리키는 최신 커밋과 워킹 디렉토리 간의 차이를 보여줌

## 스테이징된 변경사항 보기

### git diff

- **git diff —staged** or **—cached** will list the changes between the staging area and our last commit.
- “Show me what will be included in my commit if I run git commit right now”

## 특정 파일 Diff하기

### Diff-ing Specific Files

- We can view the changes within a specific file by providing git diff with a filename.

## 브랜치 전반에 걸쳐 변경사항 비교하기

### Comparing Branches

- **git diff branch1..branch2** will list the changes between the tips of branch1 and branch2

## 커밋 전반에 걸쳐 변경사항 비교하기

### Comparing Commits

- To compare two commits, provide git diff with the commit hashes of the commits in question.

# 섹션9: 스태시(Stash)의 모든 것

## Git Stash가 필요한 이유

### Git Stash

- sub branch에서 파일을 수정하고 staging이나 commit 없이 main branch로 switch 하려는 상황
  - git이 잠재적 충돌을 감지하는 경우 → commit이나 stash 이후에 이동해달라고 경고문
  - 잠재적 충돌이 잡히지 않는 경우 → 변경사항을 든 채로 branch 이동이 됨 → 그 상황에서 staging과 commit을 할 경우 빨리감기 병합이 진행

## 스태시의 기초: Git Stash Save 및 Pop

### Stashing

- Git provides an easy way of stashing these uncommitted changes so that we can return to them later, without having to make unnecessary commits.
- 커밋하지 않은 변경사항을 임시 저장하도록 해주고 불필요한 커밋으로 이력이 지저분해지는 것 없이 나중에 돌아올 수 있게 해줌

### Git Stash

- **git stash** is super useful command that helps you save changes that you are not yet ready to commit. You can stash changees and then come back to them later.
- Running git stash will take all uncommited changes (staged and unstaged) and stash them, reverting the change in your working copy.
- Use **git stash pop** to remove the most recently stashed changes in your stash and re-apply them to your working copy.

## Git Stash 불러오기

### Stash Apply

- You can use **git stash apply** to apply whatever is stashed away, without removing it from the stash. This can be useful if you want to apply stashed changes to multiple branches.
- 하지만 대부분 git stash와 git stash pop
- stash pop은 불러오고 삭제, stash apply는 불러오고 그대로 유지

## 여러 개의 스태시로 작업하기

### Stashing Multiple Times

- You can add multiple stashes onto the stack of stashes. They will all be stashed in the order you added them.
- git stash list를 통해서 여러 개의 스태시를 확인할 수 있으나 일반적으로는 하나의 스태시를 git stash를 통해 저장하고 git stash pop을 통해 불러오고 삭제

## 스태시 드롭 및 삭제

- git stash drop stash@{0} → 특정 스태시 삭제
- git stash clear → 전체 스태시 삭제
- 스태시를 들고 있는 상태에서 git stash pop이나 git stash apply를 통해 가져오지 않고 삭제하면 변경사항도 같이 사라짐

# 섹션10: 변경사항 취소하기 및 시간 여행

## 이전 커밋 확인하기(Undoing Stuff & Time Traveling)

### Checkout

- The **git checkout** command is like a Git Swiss Army knife. Many developers think it is overloaded, which is what lead to the addition of the **git switch** and **git restore** commands.
- We can use **checkout** to create branches, switch to new branches, restore files, and undo history!
- git checkout이 너무 많은 기능을 담당한다고 생각해서 git switch나 git restore 같은 새로운 명령어가 추가되었음
- **We can use git checkout commit <commit-hash> to view a previous commit.**
- Remember, you can use the **git log** command to view commit hashes. We just need the first 7 digits of a commit hash.
- Don’t panic when you see the following message…
- **일반적으로 HEAD는 특정 브랜치를 가리키지, HEAD는 별도의 커밋을 가리키지 않음 → 이전 커밋으로 체크아웃하고자 하면 헤드가 해당 커밋을 참조하도록 바꾸는 DETACHED HEAD!가 발생하는 것**

## 분리된 HEAD 재연결!

### Detached HEAD - Don’t panic when this happens! It’s not a bod thing!

You have a couple options:

1. Stay in detached HEAD to examine the contents of the old commit. Poke around, view the files, etc.
2. Leave and go back to wherever you were before - reattach the HEAD
3. Create a new branch and switch to it. You can now make and save changes, since HEAD is no longer detached.

## HEAD와 관련된 커밋 참조하기

### Checkout

- git checkout supports a slightly add syntax for referencing previous commits relative to a particular commit.
  ```
  HEAD~1 refers to the commit before HEAD (parent)
  HEAD~2 refers to 2 commits before HEAD (grandparent)
  ```
- This is not essential, but I wanted to mention it because it’s quite weird looking if you’ve never seen it.
- **별도의 해시 정보 없이 바로 이전 커밋으로 checkout 가능한 HEAD~1**

## Git Checkout으로 변경사항 폐기하기

### Discarding Changes

- Suppose you’ve made some changes to a file but don’t want to keep them. To revert the file back to whatever it looked like when you last committed, you can use:
- **git checkout HEAD <filename>** to discard any changes in that file, reverting back to the HEAD.

### Another Option

- Here’s another shorter option to revert a file…
- Rather than typing HEAD, you can substitute — followed by the file(s) you want to restore.

## Git Restore로 수정 사항 취소하기

### Restore

- **git restore** is a brand new Git command that helps with undoing operations.
- Because it is so new, most of the existing Git tutorials and books do not mention it, but it is worth knowing!
- Recall that **git checkout** does a million different things, which many git users find very confusing. **git restore** was introduced alongside **git switch** as alternatives to some of the uses for **checkout**.
- git switch와 git restore는 git checkout이 하는 일을 나누려고 도입됨
- git restore는 상단에 git checkout HEAD <filename>이 하던 discard 기능을 함

## Git Restore 변경사항 스테이징 취소하기

### Unstaging Files with Restore

- If you have accidentally added a file to your staging area with **git add** and you don’t wish to include it in the next commit, you can use **git restore** to remove it from staging.
- Use the **—staged** option like this: **git restore —staged app.js**

## Git Reset으로 커밋 취소하기

### Git Reset

- Suppose you’ve just made a couple of commits on the master branch, but you actually meant to make them on a separate branch instead. To undo those commits, you can use **git reset**.
- **git reset <commit-hash>** will reset the repo back to a specific commit. The commits are gone
- 간단하게 말하면, 현재 저장 사항을 그대로 유지하고 싶고 불필요한 커밋들을 날리고 싶을 때 그 날리고 싶은 커밋 이전의 커밋해시를 찾아서 입력 → 저장사항은 유지하되 그 사이의 커밋은 삭제되고 해당 저장사항은 staging에 들고 있음

### Reset —hard

- If you want to undo both the commits AND the actual changes in your files, you can use the **—hard** option.
- for example, **git reset —hard HEAD~1** will delete the last commit and associated changes.

## Git Revert로 커밋 원래대로 복구하기

### Git Revert

- **git revert** is similar to **git reset** in that they both “undo” changes, but they accomplish it in different ways.
- **git reset** actually moves the branch pointer backwards, eliminating commits.
- **git revert** instead creates a brand new commit which reverses / undos the changes from a commit, Because it results in a new commit, you will be prompted to enter a commit message.
- 이건 기본적으로 변경사항을 실행 취소하는 새로운 커밋을 만드는 것 → 일반적으로 협업 시 git revert를 사용

### Which One Should I Use?

- Both **git reset** and **git revert** help us reverse changes, but there is a significant difference when it comes to collaboration (which we have yet to discuss but is coming up soon!)
- **If you wnat to reverse some commits that other people already have on their machines, you should use revert.**
- If you want to reverse commits that you haven’t shared with others, use reset and no one will ever know.

# 섹션11: Github의 기초

## Github는 어떤 역할을 할까?

### What Is Github?

- Github is a hosting platform for git repositories. You can put your own Git repos on Github and access them from anywhere and share them with people around the world.
- Beyond hosting repos, Github also provides additional collaboration features that are not native to Git (but are super useful). Basically, Github helps people share and collaborate on repos.

## Git Clone으로 Github Repo 복제하기

### Cloning

- So far we’ve created our own Git repositories from scratch, but often we want to get a **local copy of an existing repository** instead.
- To do this, we can clone a remote repository hosted on Github or similar websites. All we need is a URL that we can tell Git to clone for use.

## Github 설정: SSH 구성

### SSH Keys

- You need to be authenicated on Github to do certain operations, like pushing up code from your local machine. Your terminal will prompt you every single time for your Github email and password, unless…
- You generate and configure an SSH key! Once configured, you can connect to Github without having to supply your username / password.

## Git Remote 집중 학습

### Remote

- Before we can push anything up to Github, we need to tell Git about our remote repository on Github. We need to setup a “destination” to push up to.
- In Git, we refer to these “destinations” as remotes. Each remote is simply a URL where a hosted repository lives.

### Viewing Remotes

- To view any existing remotes for you repository, we can run **git remote** or **git remote -v** (verbose, for more info)
- This just displays a list of remotes. If you haven’t added any remotes yet, you won’t see anything!
