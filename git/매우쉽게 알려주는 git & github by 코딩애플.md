### git add, commit 등으로 파일 기록해놓을 수 있음

- git add filename.txt → 특정 파일(fillname.txt)을 git add
- git commit -m ‘edit’ → 메세지와 함께 git commit
- git status → 현재 상태를 알려줌
- git log → commit한 내용들을 알려줌
- git diff → 버전 비교
    - git log와 git diff처럼 조작이 필요한 경우, j, k 스크롤 이동, q 누르면 빠져나옴
- git diff 자체는 보기가 힘들기 때문에 git difftool → git difftool abs1235 esb0941

### git branch 만들기

- git branch coupon → coupon이라는 branch 생성
- git switch coupon → 해당 branch로 이동
- git merge 머지하고싶은브랜치

### 다양한 merge 방법 (3-way, fast-forward, squash, rebase)