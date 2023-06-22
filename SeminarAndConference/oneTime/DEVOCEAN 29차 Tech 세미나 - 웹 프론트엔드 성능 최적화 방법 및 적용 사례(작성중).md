# 세션1 웹 프론트엔드 성능 최적화를 해야 하는 당위성

## Introduction

### 이번 세미나에서 알아 가셨으면 하는 것

- 웹 프론트엔드 성능 최적화하는 이유
- 웹 프론트엔드 성능 최적화 방법

### 서비스 관점에서도 웹 성능 최적화는 중요합니다

- 사용자 이탈율과 직접적인 연관이 되어 있는 최적화

## Why should Web Performance be optimized?

### How long can you wait?

- 이탈율은 비즈니스 퍼포먼스와 연관이 강하다

### Core Web Vitals(CWV) - 사용자 관점에서 정의

- LCP(Largest Contentful Paint) for measuring the loading speed
    - 웹사이트의 7할 이상은 LCP에 해당되는 건 이미지
- FID(First Input Delay) for scoring interactivity
    - 첫 번째 사용자 인풋에 얼마나 빨리 반응하느냐
- CLS(Cumulative Layout Shift) for calculating the visual stability
    - 비주얼이 얼마나 안정적이냐, 소위 말하는 덜컹거림
- FID → INP로 대체됨

### Mobile website experience affects All marketing channels

- SEO와 함께 CWV도 랭크에 반영됨

## How to measure Web Performance?

### Tools that measure Core Web Vitals

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb8f1b08-9a6d-4c0c-8d46-2105e8395e18/Untitled.png)

### Measuring Real User Monitoring (RUM) tools - 캐시 등을 포함한 실제 유저를 대상으로 하는RUM 데이터

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/01689f66-4610-4202-8957-410ff7a73ee0/Untitled.png)

### Page speed audit reports

- Lighthouse → 측정할 때마다 수치를 변할 수 있음, 하드웨어 상태에 따라

웹페이지테스트 by 캐치포인트, 페이지스피드 / web-vitals JS

### LCP optimization

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8aa79ebe-5b89-4c57-b1d7-946861024a67/Untitled.png)

### CLS optimization

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b4d33305-e06f-43b0-b6b1-52b0878508e5/Untitled.png)

### FID optimization

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5c16bed4-b2bd-41cb-8929-ff3f0a88ce70/Untitled.png)

# 세션2 ifland 웹 프론트엔드 성능 최적화 적용 사례

## How to optimize web performance at ifland studio

### Case: How to optimize web performance of ifland studio

# 기타 메모

- 웹사이트의 성향과 사용자 목적에 따라 최적화의 방향성을 설정할 수 있음
- 아마존 같은 경우 저해상도 이미지로 랜더링하고 완료되면 고해상도 이미지로 교체하는 방식으로 최적화함 → 구글 성능 지표 체크할 때 저해상도 썸네일이 일단 뜨면 로드된 걸로 체크함 → 이미지가 많은 웹사이트에서 많이 채택하는 방식

# 후기

- 일전에 정찬명님 강의를 듣고 접했던 개념들인데 이렇게 더 세부적으로 다뤄본 적은 처음인 거 같다.
- 최근에 접했던 세션 중에 가장 인상적으로 와닿았던 세션이었다. 사실 아무리 좋은 세션이어도 내가 해당 세션을 이해할 만한 수준이 되지 못해서 온전히 이해하지 못하면 와닿지는 않는 편인데, 최적화 부분은 퍼블리싱을 하면서도 접근해보려 했던 주제면서 동시에 심화적으로 해보고 싶었던 분야라 더더욱 집중해서 들을 수 있었다.
- 세션의 흐름도 좋았다. 첫 번째 세션에서 왜 해야 하는가, 어떻게 해야 하는가를 연이어 설명하고, 두 번째 세션에는 실질적으로 현업에서 어떻게 적용했는가를 접하면서 이해하는 데에 수월했다. 물론 내가 아직은 이해할 수 없는 영역도 있었지만 내 수준에서 흡수할 수 있을 만한 것들을 온전히 이해하는 데에 집중했고 그 점에서 만족스러웠다.
- 해당 세션은 영상과 발표 자료가 제공된다고 하니 자료가 올라오면 다시 정리해둘 셈이다.