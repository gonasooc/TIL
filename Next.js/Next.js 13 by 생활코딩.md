## \***\*설치와 실행\*\***

- `npx create-next-app@lastest .`

## 샘플앱 세탁

- `layout.js`에 `children`에는 `page.js`에서 return 값을 뿌려주는 구조

## 빌드와 배포

- 크롬 개발자도구의 Network 탭을 보면 해당 페이지를 로드할 때 가져오는 resources가 확인 가능함 (ex) 7.0 MB resources)
- 실서버에 사용할 코드는 좀 더 용량을 줄이고 최적화된 코드가 필요함 → 배포 버전
- `npm run build`를 통해 build하고 `npm run start`를 통해서 build된 결과물을 실행해보면 용량이 줄어들고 최적화됐다는 걸 확인 가능함(7.0 MB resources → 906 KB resources)

## 뼈대 만들기

![Untitled](../assets/36a2bd2bc49a.png)

- Next.js 13의 App router 구조에서는 라우팅 path에 따라서 해당 페이지를 찾는 구조로 되어 있음
  - 예컨대 ‘create’라는 segment가 있다면 app 폴더 밑에 ‘create’ 폴더가 있는지 찾음
  - ‘create’ 폴더 안에 약속된 page.js 파일을 찾음
  - 같은 경로에 약속된 layout.js 파일을 찾아서 children으로 랜더링
  - 같은 경로에 약속된 layout.js가 없다면 그 부모 요소에 layout.js를 찾아서 children으로 랜더링

## Single Page Application

- SSR의 단점이라고 할 수 있는데, 페이지의 일부 영역만 바뀌거나 이미 방문한 페이지를 다시 방문하더라도 전체 페이지를 다시 ssr을 통해 클라이언트로 가져옴 → 사용자 입장에선 느리다고 느낄 것이고 서비스를 제공하는 입장에선 돈이 많이 듬
- a tag 대신 <Link />를 사용함으로 SPA처럼 구성할 수 있음 → <Link />를 사용하면 사용자가 클릭하기도 전에 fetch를 하고, 이미 방문했던 페이지를 다시 방문하려고 하면 서버와 통신하지 않음
- 아래 3가지를 충족시켜줌
  - 링크를 클릭하면 페이지 전체 리로딩이 일어나지 않고 필요한 콘텐츠만 로딩하고 싶다.
  - 이미 방문한 페이지는 캐슁을 해서 다시 다운로드 하지 않도록 하고 싶다.
  - 미리 페이지를 로드했다가 실제 요청이 있을 때 클라이언트 측에서 즉시 응답한다.
- Link는 Next.js에서 SPA를 매우 쉽게 구현하도록 도와주는 도구

## **backend**

- json-server를 활용한 실습

  - `npx json-server --port 9999 --watch db.json`
  - db.json에 활용하고 싶으면 json data를 작성하고 저장하면 watch 때문에 재시작함

    ```json
    {
      "topics": [
        {
          "id": 1,
          "title": "html",
          "body": "html is ..."
        },
        {
          "id": 2,
          "title": "css",
          "body": "css is ..."
        }
      ],

      "posts": [
        {
          "id": 1,
          "title": "json-server",
          "author": "typicode"
        }
      ],
      "comments": [
        {
          "id": 1,
          "body": "some comment",
          "postId": 1
        }
      ],
      "profile": {
        "name": "typicode"
      }
    }
    ```

  - `fetch('http://localhost:9999/topics').then((response) => {return response.json()}).then(result => {console.log(result)});`

  ## **글 목록 가져오기**
