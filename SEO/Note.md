# 공식문서

### 네이버

[검색엔진 최적화의 목적](https://searchadvisor.naver.com/guide/seo-basic-intro)

### 구글

[SEO 기본 가이드: 기본사항 | Google 검색 센터 | 문서 | Google Developers](https://developers.google.com/search/docs/fundamentals/seo-starter-guide?hl=ko)

### **robots.txt 관련**

[robots.txt 설정하기](https://searchadvisor.naver.com/guide/seo-basic-robots)

(예) 모든 검색엔진의 로봇에 대하여 접근 가능하게 설정

```
User-agent: *
Allow: /
```

### **네이버 마크업 가이드**

[콘텐츠 마크업](https://searchadvisor.naver.com/guide/markup-content)

### **meta name=”robots” → 별도 선언하지 않으면 content=”index,follow”**

[Defaults for robots meta tag](https://stackoverflow.com/questions/24165589/defaults-for-robots-meta-tag)

### 네이버 **SEO 간단 체크**

[[네이버: 로그인]](https://searchadvisor.naver.com/tools/sitecheck)

# 구글 SEO 관련

## 인트로

- 구글 검색에서 최상단 2~3개 정도는 광고 지면의 영역
- 그 나머지 영역을 자연 검색, 오가닉이라고 표현하는데, 이걸 SEO로 할 수 있는 영역
- 사용자 입장에선 상단에 있는 것이 광고라는 걸 인지하고 있기 때문에 클릭하지 않는 성향이 있음
- 실제로 광고 클릭 비율은 2% 정도, 오가닉-자연 검색 컨텐츠는 40% 정도 된다고 함
- 오가닉을 통해서 들어온 사용자는 광고 대비 전환율 또한 높음
- SEO를 통해 확보한 트래픽은 지속 가능한 요소

## 검색 엔진 구성요소 세 가지

### 크롤링

- 검색 로봇 크롤러

### 인덱싱

- 크롤러가 모아준 문서를 데이터베이스에 저장하는 프로세스

### 랭킹

- SERP(Search Engine Result Page)
- 전체 고객의 대부분은 첫 페이지에서 두 번째 페이지로 넘어가지 않음
- SERP 올라가는 방법
    1. 사이트 보안(ex) https)
    2. 모바일 친화적
    3. 페이지 스피드
- 키워드 찾는 방법
    - Google Keyword Planner
    - 네이버 광고 센터
- 키워드 관련
    - [keywordtool.io](http://keywordtool.io/)
    - 대상 키워드를 먹기 위해선 의도 키워드를 사용해서 잡아 나가야 함
    - GA나 구글 서치 콘솔을 통해서 유입된 키워드를 체크할 수 있음
    - [https://ahrefs.com/](https://ahrefs.com/)
- SEO 컨텐츠 작성할 때, 중요한 3가지
    - 타이틀 태그
        - 영문 기준 50~60자 정도
        - 국문 기준 25~30자 정도
    - 메타 태그
        - 디스크립션은 크롤링에는 영향을 주지 않지만 클릭율에 영향을 줌
    - 헤드 태그
        - h1, h2, h3…
    

[구글 SEO 최적화하는 3가지 방법🤟 (검색엔진최적화) 광고비 없이 상위노출 해보자! #1](https://www.youtube.com/watch?v=5evFsqf7NHE)

## SEO 컨텐츠 작성 순서

### 키워드

- 컨텐츠의 목표
- [https://keywordtool.io/](https://keywordtool.io/) 으로 체크하고 키워드 선정
- 예컨대 Search Volume이 적은데 Competition, 경쟁력 지수가 너무 크다면 굳이 그 키워드에 힘을 쏟을 필요가 없음 → 반대로 Search Volume은 큰데 Competition이 낮다면 꿀키워드 → 그 기준을 잡는 것이 중요
- 관련 키워드들도 같이 선정

### 경쟁사 분석

- [https://ahrefs.com/](https://ahrefs.com/)
- DR(Domain Rating), Backlinks, Keywords들을 중점적으로 분석해서 큰 그림 잡기

### 컨텐츠 구성

- URL
- 타이틀
- 헤딩 태그
- 텍스트
- 이미지 태그

### 컨텐츠 작성

- 컨텐츠 구성 5가지에 앞서 잡아놓았던 키워드가 많이 들어가면 들어갈수록 좋음
- 헤딩 태그에 키워드를 녹이고 img에 alt나 title 같은 것도 같이 챙겨주면 좋음

### 인덱싱

- 구글 서치 콘솔 - URL 검사 - 색인 생성 요청
- 1~2주 안에 색인 요청 완료

### 모니터링

- serp에서 계속 모니터링하면서 컨텐츠 워싱과 업데이트 필요
- serp에서 너무 밀려나면 다른 컨텐츠들을 merge해서 다시 작성해보는 것도 좋음

[SEO란? 구글 SEO 최적화 방법! 🤟 키워드 선정부터 메타 태그까지 전부 알려드립니다! #2](https://www.youtube.com/watch?v=zd6d4kuNJA8)