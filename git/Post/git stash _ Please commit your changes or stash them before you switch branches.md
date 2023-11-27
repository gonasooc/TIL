## Git Stash가 필요한 이유

### Git Stash

- 작업을 진행하던 branch에서 파일을 수정하고 staging이나 commit없이 다른 branch로 `git switch branch-name`나 `git checkout branch-name` 하려고 하면 아래와 같은 에러를 만나게 됩니다.
  - git이 잠재적 충돌을 감지하는 경우 → **말 그대로 commit이나 stash 이후에 이동해달라고 경고문**
    ```
    error: Your local changes to the following files would be overwritten by checkout:
    ...
    Please commit your changes or stash them before you switch branches.
    Aborting
    ```
  - 잠재적 충돌이 잡히지 않는 경우 → 변경사항을 든 채로 branch 이동이 됨 → 그 상황에서 staging과 commit을 할 경우 빨리감기 병합이 진행
- 해당 작업한 내용을 commit한 다음에 다시 이동하면 되겠지만,
  ```
  git add .;git commit -m 'commit message'
  git switch develop
  ```
- 유의미한 commit이라면 상관 없으나, 작업 중간에 branch를 새로 따고 싶거나 현재까지가 유의미한 commit 시점이 아닌 경우 불필요한 commit을 남기게 됩니다.
- 이때 사용 가능한 게 `git stash`입니다.
  ```
  git stash
  git switch -c feature/new
  git stash pop
  ```
  - `git stash` - 현재까지의 수정사항을 임시 저장합니다. (저장 후에 `git status`로 체크하면 `nothing to commit, working tree clean` 출력)
  - `git switch -c feature/new` - 현재의 branch를 기준으로 feature/new branch를 생성합니다.
  - `git stash pop` - 가장 최근의 stash를 불러오고 해당 stash는 바로 삭제합니다.
  - 그래서 주로 사용되는 키워드는 `git stash`와 `git stash pop`이라고 볼 수 있습니다.

## 스태시의 기초: Git Stash Save 및 Pop

### Stashing

- commit 하지 않은 변경사항을 임시 저장하도록 해주고, 불필요한 commit으로 이력이 지저분해지는 것 없이 나중에 돌아올 수 있게 해주는 기능이라고 볼 수 있습니다.

### Git Stash

- `git stash`는 아직 커밋할 준비가 되지 않은 변경사항을 저장할 수 있도록 도와주는 매우 유용한 명령어입니다. 변경사항을 저장한 후 나중에 다시 변경사항으로 돌아올 수 있습니다.
- `git stash pop`을 사용하여 stash의 가장 최근에 저장된 변경 사항을 제거하고 작업 영역에 다시 적용합니다.

## Git Stash 불러오기

### Stash Apply

- `git stash pop`과는 다르게 `git stash apply`를 사용하면 stash list에서 제거하지 않고 stashed된 모든 것을 적용할 수 있습니다. 이는 특정 stash를 여러 곳에 적용하고자 할 때 유용하게 사용 가능합니다.
- 하지만 대부분의 작업은 `git stash`와 `git stash pop`를 주로 사용합니다.
- `git stash pop`과 `git stash apply`의 차이 - `git stash pop`은 불러오고 삭제되고, `git stash apply`는 불러오고 stash list는 그대로 유지됩니다.

## 여러 개의 스태시로 작업하기

### Stashing Multiple Times

- 여러 개의 stash를 stash stack에 추가할 수 있습니다. 추가한 순서대로 모두 stash됩니다.
- `git stash list`를 통해서 여러 개의 stash를 확인할 수 있으나, 일반적으로는 하나의 stash를 `git stash`를 통해 저장하고 `git stash pop`을 통해 불러오고 삭제하는 작업을 주로 사용합니다.

## 스태시 드롭 및 삭제

- `git stash drop stash@{0}` → 특정 stash 삭제
- `git stash clear` → 전체 stash 삭제
- 스태시를 들고 있는 상태에서 `git stash pop`이나 `git stash apply`를 통해 가져오지 않고 삭제하면 변경사항도 같이 사라지게 됩니다.

## 참고자료

- https://www.udemy.com/course/best-git-github/
