### 작업하던 local 작업본과 해당 branch 그대로 들고 가서 새로운 branch 생성

- 실수로 다른 branch에서 작업했을 때 유용

```jsx
git checkout -b new_branch old_branch
```

### **git clone**

1. 레파지토리 주소 복사
2. vs code에서 ctrl + shift + p 실행
3. git clone 입력 후 실행
4. 내 git repository 주소.git
5. 기존에 repository 저장했던 폴더 내 경로로 설정

### **주요 git 명령어 정리**

```
# Git commit 취소하는 명령어
git reset —soft HEAD^

# 원격 저장소 처음 설정
git remote add origin ...

# 원격 저장소에 처음 푸시
git push -u origin master

# 현재 원격 저장소 보기
git remote -v

# 원격 저장소 변경
git remote set-url origin ...

# 모든 변경사항 전부 되돌리기
git reset --hard

# 파일 하나만 되돌리기
git checkout HEAD -- my-file.txt

# 현재 변경사항 커밋 메세지와 함께 커밋
git commit -am "commit message"

# 현재 커밋 보기
git show --oneline -s

# 파일 권한 변경 무시
git config core.filemode false

# 버퍼 크기 늘리기
git config --global http.postBuffer 1048576000

# 사용자 정보 입력
git config --global user.name "pocketcompany"
git config --global user.email "pocket.company@gmail.com"
```

### **gitlab에 프로젝트 생성**

1. 프로젝트명-pb, 프로젝트명-landing 식으로 명시적으로 표기
2. 생성 후 git init을 통해 만들고
3. 개발팀 멤버 전체 초대하고
4. setting → repository → protected branches에서 Allowed to merge, Allowed to push를 developer + maintainer로 권한 변경

### **gitlab에 저장소 빠르게 변경하기**

- 기존 저장소에서 새 저장소로 경로 변경한 후 바로 origin에 push
    1. git remote set-url origin [https://git.pocketcompany.dscloud.biz/pocket-dev/tarming-web.git](https://git.pocketcompany.dscloud.biz/pocket-dev/tarming-web.git)
    2. git push -u origin master