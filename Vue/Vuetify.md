# 개요

- Vue 기반 UI 프레임워크
- 구글의 Material Design 기반 설계
- Vue CLI를 통한 프로젝트 생성 → Vue Router 설치 → Vuetify 설치
- npm install -g @vue/cli → Vue CLI 설치
- vue create 프로젝트명 → vue CLI 세팅
- npm i -S vue-router → Vue router 설치
- 페이지 구성은 src/views/페이지 생성
- router 설정은 src/router/index.js
- App.vue에 <router-view />로 해당 router 경로가 뿌려질 영역 넣어줘야 함
- vue add vuetify → vuetify 설치

# 프로젝트 구조 설명

## package.json

- name: package의 이름
- version: package 버전 정보 → 앞에서부터 메이저, 마이너, 패치 버전 정보를 뜻함
- private → package 공개 여부
- scripts → 이 package에서 사용하고자 하는 명령어 정의
- npm run build → dist 폴더에 빌드
- npm run lint → 자동으로 문법 수정

[List of available rules](https://eslint.org/docs/rules/)

- browserslist

[GitHub - browserslist/browserslist: 🦔 Share target browsers between different front-end tools, like Autoprefixer, Stylelint and babel-preset-env](https://github.com/browserslist/browserslist)

- editorconfig

[EditorConfig](https://editorconfig.org/)

- eslintrc.js

[GitHub - vuetifyjs/eslint-plugin-vuetify: An eslint plugin for Vuetify](https://github.com/vuetifyjs/eslint-plugin-vuetify)