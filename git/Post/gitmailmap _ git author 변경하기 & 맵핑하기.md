## git 초기 설정

### git config

- 오래 전에 git을 처음 설치했을 때 초기 설정을 하면서 user.name과 user.email을 global로 설정하는 경우가 많습니다.

    ```
    git config --global user.name "Joel"
    git config --global user.email joel@vlending.co.kr
    ```

- Sourcetree나 GitKraken 같은 GUI 툴은 초기 설정에서 질문을 통해 이 값을 저장합니다.

### 프로젝트마다 author를 다르게

- 해당 프로젝트의 루트로 가서(.git 폴더가 존재하는 경로) 똑같이 git config를 재설정해주면 됩니다. 전체가 아니라 해당 프로젝트의 config 정보만 바꾸는 것이기 때문에 global은 제외합니다.

    ```
    git config user.name "gonasooc"
    git config user.email doupler@gmail.com
    ```


## 원치 않는 author

### Human Error

- 상단의 예시처럼 global 키워드를 통해 user.name은 Joel, user.email은 joel@vlending.co.kr로 설정해뒀는데, 회사 아이디가 아닌 개인 아이디로 commit을 하고 싶었다고 가정해봅시다. 총 3번의 commit을 진행한 상황이라고 가정하고 git log로 commit log를 봅시다.

    ```
    commit 9b3e1a56af28ea750c95106ef642e175d196c770 (HEAD -> main, origin/main)
    Author: gonasooc <doupler@gmail.com>
    Date:   Wed Nov 29 17:57:22 2023 +0900
    
        commit 3
    
    commit 4bdcc3fdbc006ec11c02e02f9117e3ba35c15343
    Author: Joel <joel@vlending.co.kr>
    Date:   Wed Nov 29 17:56:38 2023 +0900
    
        commit 2
    
    commit 776602a1a6245443cb7240b7353f7bcbb4af7d81
    Author: Joel <joel@vlending.co.kr>
    Date:   Wed Nov 29 17:55:45 2023 +0900
    
        commit 1
    ```

- global로 설정한 git config 수정을 잊고, 이미 앞서 두 번의 commit은 원치 않게 회사 author로 commit, push를 해버린 상황이면 난감해집니다.
- 또한 github 정책상 git에서 사용하는 user.email과 github 아이디가 동일해야 잔디가 심어지고 기여도 체크가 됩니다. (user.name은 상관 없음)

### git rebase

- 실수한 commit이 마지막 commit인 경우 + 개인 프로젝트라 commit이 조금 꼬여도 괜찮은 경우는 아래 링크를 통해서 git rebase를 시도해보는 것도 괜찮은 거 같습니다.

  [git commit 후 author, email을 수정하는 방법 - 코드도사](https://codedosa.com/1856)

- 다만 마지막 commit도 아니고 팀 프로젝트인 경우에는 애초에 다른 사람의 commit에 영향을 줄 수 있기 때문에 안전한 방법 중 하나는 gitmailmap입니다.

### gitmailmap

- gitmailmap을 통해 commit을 수정하거나 잔디를 재차 심을 순 없습니다. 다만 맵핑을 통해 git log 관리가 가능합니다.
- 말 그대로 기존에 올라간 author 정보를 새로 작성한 정보와 맵핑해서 새로운 정보로 출력합니다.
- 우선 아까 git log를 다시 한번 봅시다.

    ```
    commit 9b3e1a56af28ea750c95106ef642e175d196c770 (HEAD -> main, origin/main)
    Author: gonasooc <doupler@gmail.com>
    Date:   Wed Nov 29 17:57:22 2023 +0900
    
        commit 3
    
    commit 4bdcc3fdbc006ec11c02e02f9117e3ba35c15343
    Author: Joel <joel@vlending.co.kr>
    Date:   Wed Nov 29 17:56:38 2023 +0900
    
        commit 2
    
    commit 776602a1a6245443cb7240b7353f7bcbb4af7d81
    Author: Joel <joel@vlending.co.kr>
    Date:   Wed Nov 29 17:55:45 2023 +0900
    
        commit 1
    ```

- 루트에 .mailmap 파일을 생성합니다. 예시로 작성한 설정은 아래와 같습니다. (# 뒤는 주석 처리)

    ```
    gonasooc <doupler@gmail.com> Joel <joel@vlending.co.kr>
    # <doupler@gmail.com> <joel@vlending.co.kr>
    # gonasooc <doupler@gmail.com> <joel@vlending.co.kr>
    ```

- 첫 번째 줄을 주석 해제해서 적용했을 때 git log → user.name과 [user.email](http://user.email) 모두 맵핑해서 gonasooc <doupler@gmail.com>로 출력합니다.

    ```
    commit 9b3e1a56af28ea750c95106ef642e175d196c770 (HEAD -> main, origin/main)
    Author: gonasooc <doupler@gmail.com>
    Date:   Wed Nov 29 17:57:22 2023 +0900
    
        commit 3
    
    commit 4bdcc3fdbc006ec11c02e02f9117e3ba35c15343
    Author: gonasooc <doupler@gmail.com>
    Date:   Wed Nov 29 17:56:38 2023 +0900
    
        commit 2
    
    commit 776602a1a6245443cb7240b7353f7bcbb4af7d81
    Author: gonasooc <doupler@gmail.com>
    Date:   Wed Nov 29 17:55:45 2023 +0900
    
        commit 1
    ```

- 두 번째 줄을 적용했을 땐, user.email만 명시되어 있기 때문에 user.name은 그대로고 user.email만 맵핑해서 출력합니다.

    ```
    # gonasooc <doupler@gmail.com> Joel <joel@vlending.co.kr>
    <doupler@gmail.com> <joel@vlending.co.kr>
    # gonasooc <doupler@gmail.com> <joel@vlending.co.kr>
    ```

    ```
    commit 9b3e1a56af28ea750c95106ef642e175d196c770 (HEAD -> main, origin/main)
    Author: gonasooc <doupler@gmail.com>
    Date:   Wed Nov 29 17:57:22 2023 +0900
    
        commit 3
    
    commit 4bdcc3fdbc006ec11c02e02f9117e3ba35c15343
    Author: Joel <doupler@gmail.com>
    Date:   Wed Nov 29 17:56:38 2023 +0900
    
        commit 2
    
    commit 776602a1a6245443cb7240b7353f7bcbb4af7d81
    Author: Joel <doupler@gmail.com>
    Date:   Wed Nov 29 17:55:45 2023 +0900
    
        commit 1
    ```


## 참고자료

- https://git-scm.com/docs/gitmailmap#_examples