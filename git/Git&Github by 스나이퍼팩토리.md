# 1강 Git과 Github란

## Git이란?

- 파일의 히스토리를 관리해주는 시스템
    - 2023년 1월 1일, README.md의 좋습니다. 를 나쁩니다. 로 변경했다.
    - 2023년 1월 2일, README.md의 나쁩니다. 를 좋습니다. 로 변경했다.

## Github란?

- 클라우드 기반의 Git 저장소(Git이 관리하는 파일들) 호스팅 서비스

# 2강 Repository

## Repository(레포지토리, 저장소)

- Git으로 관리하는 프로젝트 폴더

# 3강 commit

## commit(커밋)

- Git이 코드의 변화를 기록하는 것
- 소스코드의 변경사항을 한 단위로 묶어 기록하는 것
- 쉽게 생객하면, 타임라인을 만드는 것

## 세 가지 상태

- Working Directory: 수정했지만, 아직 commit 되지 않은 상태
- Staging Area: 수정된 파일을 다음 commit에 포함할 것을 표시한 상태
- Repository: commit된 상태

# 4강 push, pull

## Origin(=Remote Repository)

- Github에 업로드된 레포지토리
- Git Graph에서 `origin`으로 표기된 부분이 어디까지 remote repository에 올라갔는지에 대해 표기

## Push와 Pull

- Origin과 동기화하는 동작
- Push → local의 앞선 변경사항을 origin에 반영
- Pull → origin의 앞선 변경사항을 local에 반영

# 5강 branch

## branch란?

- 소스코드의 복사본을 만드는 것
- 특정 커밋에서, 새로운 버전으로 분기하는 것

# 6강 merge, conflict

## Merge

- 하나의 branch를 다른 branch에 합치는 것

## Conflict

- branch를 머지할 때, 같은 라인의 소스코드를 수정한 경우 발생
- 둘 중 어느 branch의 수정사항을 받을지 결정해야 함

## 언제 사용할까요?

- 공통의 코드베이스를 공유하면서, 효율적이고 독립적인 작업 환경이 필요할 때