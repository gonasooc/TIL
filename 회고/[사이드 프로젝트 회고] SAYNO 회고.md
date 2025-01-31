## 참여 계기와 성과

작년 사이드 프로젝트 ‘[SAYNO](https://justsayno.netlify.app/)’에 참여했다. 해당 프로젝트에 대한 설명은 [비사이드의 프로젝트 페이지](https://bside.best/projects/detail/P240717090339)에 잘 요약되어 있다. 작년에 사이드 프로젝트를 하고 싶은 마음은 있었지만, 방송대 학업과 회사 업무를 병행하다 보니 짬을 내기 어려웠다. 그 무렵, 포텐데이를 통해 사이드 프로젝트를 해보자는 지인의 권유가 있었는데, FEConf 발표를 준비하는 시기와 겹쳐서 고민이 있었다. 다만 열흘이라는 컴팩트한 기간과 기회가 닿았을 때 하지 않으면 내내 못할 것 같다는 생각이 들어서 참여하게 되었다. 결과적으로 기능이나 작업에는 아쉬움이 있었지만, LLM(CLOVA X)과의 연계 프로젝트를 해봤다는 경험적 측면과 27팀 중 2등 수상이라는 성과가 있었다.

## 팀 빌딩과 주제, 기술 스택

### 팀 빌딩

최초 기획자는 지인이었으며, 추가로 컨택한 디자이너도 이전에 함께 일했던 동료였다. 백엔드 개발자는 추후 포텐데이의 슬랙 채널에서 별도로 컨택했고, 최종적으로 기획 1, 디자이너 1, 프론트엔드 1, 백엔드 1로 팀을 구성했다. 

### 최초 고려한 기술 스택

사이드 프로젝트의 장점은 회사에서 사용하지 못하던, 혹은 능숙하지 못하던 기술 스택을 맘껏 적용할 수 있다는 점이다. 특별히 새로운 스택을 도입하려고 했던 건 아니고 최근에 주로 사용하는 Next.js, TanStack Query, Zustand, React Hook Form 같은 기술을 활용하고자 했다.

### 주제 선정

오프라인으로 만나기 전에 온라인 미팅을 통해 다양한 주제를 논의했다. 당시 포텐데이 회차는 LLM(CLOVA X)과의 연계가 필수 조건이었기 때문에 주제 선정에 한계는 있었다. 끝내 선정된 주제는 거절 멘트 생성기였다. 사용자의 선택에 따라 질문을 만들고, API를 통해 적당한 거절 멘트를 추천해 주는 서비스였다.

### 최종 선택한 기술 스택

정해진 주제를 기반으로 생각해 봤을 때 고려했던 기술 스택이 오버스펙이라는 생각이 들었다. 일단 Next.js를 하기엔 프로젝트의 규모가 간소했고 특별히 SEO가 중요하지 않았다. 서버로부터 빈번하게 데이터를 받아오지 않아서 TanStack Query의 캐싱 기능이 필요하지 않았고, 사용자가 요청할 때마다 값을 새로 받아와야 하니 `staleTime` 개념도 큰 의미가 없었다. 특별히 전역 상태 관리가 필요하지도 않았다. 그래서 기술 목표를 TypeScript와 CSS Module에 익숙해지는 방향으로 전환했다. Vite를 통해 기본 개발 환경을 세팅했고, 백엔드와 저장소가 나뉘어 있긴 했지만 간단하게 `Feature`, `Update`, `Refactor` 같은 prefix로 간단히 commit convention을 맞췄다. 배포는 시간 관계상 netlify를 사용했고 `develop`, `master` branch에 맞춰 배포 환경을 분리했다.

## 주요 작업 요소

### 질문지 컴포넌트 구성

질문지는 여러 단계로 구성된 설문 형식이었다. 처음엔 모든 질문지를 고려해서 하나의 컴포넌트와 `props`로 구성하려 했다. 다만 초기에 고려하지 못한 건 라우터가 변경되어도 기존 값들을 그대로 들고 있어야 한다는 점, step을 오갔을 때 사용성이 좋아야 한다는 점이었다. 결국 컴포넌트를 상황에 맞게 ~~내똥내치~~ 개선해 나가는 데에 집중했다. 대부분의 시간은 ~~내똥내치~~ 효율적인 컴포넌트 구성에 할애했다.

### 종성 여부 판단

1개의 한글은 초성, 중성, 종성의 조합으로 만들어진다. 그리고 종성 유무에 따라 ‘을/를', '이/가', '은/는’, ‘와/과’가 달라진다. 사용자의 선택에 따라 종성이 달라질 수 있기 때문에 종성을 판단하는 로직이 필요했다. 해당 단어에서 마지막 글자를 추출하여 유니코드로 만들고, 해당 유니코드를 기준으로 어떤 종성인지 판단했다.

```tsx
export const getParticle = (word: string): string => {
  if (!word) return '';
  const lastChar = word.charCodeAt(word.length - 1);
  const hasBatchim = (lastChar - 0xac00) % 28 !== 0;
  return hasBatchim ? '을' : '를';
};
```

### 요청 제한

진입장벽을 낮추자는 의미로 로그인 기능을 제외했기 때문에 2차 개발에선 1시간에 최대 3회로 제한했다. 최초엔 서버에서 내려주는 429 에러 코드를 통해 처리하려 했으나 서버 쪽 이슈로 프론트에서 처리하게 되었다. 토큰을 주고받는 구조도 아니었고 협의할 시간이 없는 이유도 있었지만, 별도의 `uuid`를 통해 서버에서 처리하는 방식이 더 나았을 수도 있겠다는 아쉬움이 남는다.

```tsx
// 요청 한도를 설정합니다.
const REQUEST_LIMIT = 3;
const ONE_HOUR_IN_MS = 60 * 60 * 1000;
const STORAGE_KEY = 'requestCount';
const STORAGE_TIMESTAMP_KEY = 'lastRequestTimestamp';
// 1시간 경과 여부 확인
export const isOneHourPassed = (timestamp: number): boolean => {
  const now = Date.now();
  return now - timestamp > ONE_HOUR_IN_MS;
};
// 초기화
export const initializeRequestCount = (): void => {
  localStorage.setItem(STORAGE_KEY, '0');
  localStorage.setItem(STORAGE_TIMESTAMP_KEY, Date.now().toString());
};
// 현재 요청 카운트 가져옴
export const getRequestCount = (): number => {
  const count = localStorage.getItem(STORAGE_KEY);
  const lastTimestamp = localStorage.getItem(STORAGE_TIMESTAMP_KEY);
  // 별도 저장된 값이 없으면 초기화
  if (!count || !lastTimestamp) {
    initializeRequestCount();
    return 0;
  }
  const timestamp = parseInt(lastTimestamp, 10);
  // 1시간이 지났으면 초기화
  if (isOneHourPassed(timestamp)) {
    initializeRequestCount();
    return 0;
  }
  return parseInt(count, 10);
};
// 카운트 증가
export const incrementRequestCount = (): void => {
  let count = getRequestCount();
  count += 1;
  localStorage.setItem(STORAGE_KEY, count.toString());
  localStorage.setItem(STORAGE_TIMESTAMP_KEY, Date.now().toString());
};
// 요청 가능 여부 체크
export const canMakeRequest = (): boolean => {
  return getRequestCount() < REQUEST_LIMIT;
};
```

## KEEP

별도의 프로그램을 통해 사이드 프로젝트를 진행하니 확실한 동기부여가 되었다. 별도의 참가비가 있긴 하지만 중간중간 가이드의 역할을 해주기에 팀 단위의 사이드 프로젝트가 익숙하지 않은 사람에게는 좋은 프로그램이라는 생각이 든다.

CSS Module 방식의 장점을 느낄 수 있는 기회였다. 다소 익숙하지 않은 부분이 있었지만 scope의 적용을 받는 CSS Module이 각 컴포넌트와 같이 배치되어 있다 보니 수정을 위한 코드 추적이 쉬웠다. 

지속 배포를 통해 현 개발 상황을 명확히 알려주고, 그에 따른 기능 추가나 수정에 적절하게 대응했다는 생각이 든다. 열흘이라는 시간은 한정적이기 때문에 특정 기점이 아닌 지속적인 소통이 중요한 프로젝트였다. 추가로 데일리 스크럼을 제안했고 각자의 업무 진척도를 확인할 수 있었다.

## PROBLEM

중간에 기획이나 디자인이 바뀌는 경우가 있었고, 그런 경우 당연히 컴포넌트나 CSS 수정이 따르기 마련이었다. 그 과정에서 ~~내똥내치~~ 결합이 강하게 되어 있던 컴포넌트를 수정해야 하는 경우도 있었는데, 시간이 타이트하다 보니 완성도가 좋진 않았다. 

프롬프트 엔지니어가 따로 없었기 때문에 LLM을 통한 결괏값을 개선하는 데에 다소 어려움이 있었다. 열흘이라는 한정적인 시간과 더불어 CLOVA X의 성능에 대한 의심 때문에 좀 더 성실하게 개선을 해보지 못한 점이 아쉽다. 2차 개발에 Claude로 변경했지만 그렇게 유의미한 성과를 얻진 못했고, 최초 나온 결과에 설정을 변경해서 다시 결과를 받아오는 기능을 추가했지만 결괏값에 다소 이슈가 있어서 끝내 제외하게 되었다. 실제로 옵션은 많아졌지만 결과는 유의미하게 발전되지 않았다.

```tsx
// radioOptions.ts

const RADIO_OPTIONS = {
  CATEGORY_OPTIONS: ['돈', '약속', '학교', '회사', '결혼', '기타'],
  SUB_CATEGORY_OPTIONS: {
    돈: [],
    약속: [],
    학교: ['과제', '기타'],
    회사: ['회식', '업무', '기타'],
    결혼: ['청첩장 모임', '결혼식 참여', '축가 및 축사', '결혼기념일', '기타'],
    기타: [],
  },
  GENDER_OPTIONS: ['남자', '여자'],
  AGE_OPTIONS: ['10대', '20대', '30대', '40대', '50대'],
  REASON_OPTIONS: [
    {
      id: 1,
      text: '직접 입력',
    },
    {
      id: 2,
      text: '일정이 안돼서',
    },
    {
      id: 3,
      text: '돈이 없어서',
    },
    {
      id: 4,
      text: '세이노가 알아서 해줘',
    },
  ],
  STYLE_OPTIONS: ['다정하게', '직구로', '유머러스하게'],
  POLITE_OPTIONS: ['존댓말 사용', '반말 사용'],
};

export default RADIO_OPTIONS;
```

```tsx
// radioOptionsV2

const RADIO_OPTIONS_V2 = {
  CATEGORY_OPTIONS: ['직장', '일상', '학교'],
  SUB_CATEGORY_OPTIONS: {
    직장: ['업무', '회식/모임', '교육/훈련', '인사 관련', '동료 부탁', '기타'],
    일상: ['사회적 모임', '금전', '약속', '기타'],
    학교: ['과제', '스터디', '행사', '기타'],
  },
  SUB_DESC_OPTIONS: {
    업무: '추가적인 업무요청',
    '회식/모임': '참석하기 싫은',
    '교육/훈련': '참석하기 싫은',
    '인사 관련': '갑작스러운 면담',
    '동료 부탁': '갑작스러운',
    '사회적 모임': '결혼식, 동창회 등',
    금전: '곤란한',
    약속: '참석하기 싫은',
    과제: '하기 싫은',
    스터디: '참석하기 싫은',
    행사: '참석하기 싫은',
    기타: '',
    '세이노가 알아서 해줘': '마땅히 생각이 안난다면?',
    직접입력: '',
  },
  SUB_RELATIONSHIP_OPTIONS: {
    직장: [
      {
        id: 2,
        text: '직장동료',
      },
      {
        id: 3,
        text: '직장상사',
      },
      {
        id: 4,
        text: '거래처',
      },
      {
        id: 5,
        text: '대표',
      },
      {
        id: 6,
        text: '부하직원',
      },
      {
        id: 1,
        text: '직접입력',
      },
    ],
    일상: [
      {
        id: 2,
        text: '친구',
      },
      {
        id: 3,
        text: '지인',
      },
      {
        id: 4,
        text: '가족',
      },
      {
        id: 5,
        text: '모르는 사람',
      },
      {
        id: 1,
        text: '직접입력',
      },
    ],
    학교: [
      {
        id: 2,
        text: '선배',
      },
      {
        id: 3,
        text: '후배',
      },
      {
        id: 4,
        text: '동기',
      },
      {
        id: 5,
        text: '교수님/선생님',
      },
      {
        id: 1,
        text: '직접입력',
      },
    ],
  },
  GENDER_OPTIONS: ['남자', '여자'],
  AGE_OPTIONS: ['10대', '20대', '30대', '40대', '50대'],
  REASON_OPTIONS: [
    {
      id: 4,
      text: '세이노가 알아서 해줘',
    },
    {
      id: 1,
      text: '직접 입력',
    },
  ],
  STYLE_OPTIONS: ['직설적', '완곡하게', '유머러스'],
  POLITE_OPTIONS: ['존댓말', '반말'],
};

export default RADIO_OPTIONS_V2;
```

CSS Module을 좀 더 재사용성을 높이는 방향으로 짜지 못한 게 아쉽다. 익숙해지는 계기는 되었지만 좀 더 재사용성을 고려해서 짜놨다면 V2에 제대로 활용할 수도 있었을 일이었다. 물론 재사용성은 디자이너와의 협의가 전제되어야 가능한 일이기도 한데, 열흘이라는 시간은 하나의 큰 틀을 짜기엔 다소 부족한 시간이다.

하나의 함수가 여러 역할을 수행하고 있어 디버깅을 어렵게 만든 케이스였다. 처음엔 애초에 페이지가 많지 않아서 여러 함수가 아닌 단일 함수 내 분기 처리를 통해 구성해도 무방하다고 생각했지만, 간단한 분기 처리에 비해 디버깅은 간단하지 않았다.

1.0 배포 이후 설문을 통해 개선사항을 파악했을 때 기능에 대한 내용이 많았고 실제로 개발을 하면서 다른 LLM을 통한 기능 개선을 해보자는 게 추가 개발의 의도였는데, 어쩌다 보니 UI/UX 위주로 개발하게 된 부분이 다소 아쉽다. 실제로 프론트에서의 작업량은 많았지만 유의미한 개선이 되진 않았다. 누군가 명확한 키를 쥐고 방향을 제시하지 못한 점이 아쉽다.

## TRY

기획 변경에 대해 고려하자. 머리로는 알면서도 경험이 부족해서 놓치는 부분이기도 하다. 기획이 변경될 것을 고려해서 유연하게 짜놓는 태도가 필요하다.

근본적인 원인을 먼저 찾고 개선하려는 태도를 갖자. 설문을 통해 어떤 걸 개선해야 하는지는 인지했지만 개선을 위해 무얼 해야 하는지는 명확히 인지하지 못했다. LLM을 변경하고 질문지의 방향을 개선했지만 끝내 유의미한 결과를 얻지 못한 건 근본적인 원인을 제대로 파악하지 못했기 때문이라는 생각이 든다.

시간에 쫓기더라도 새로운 기술을 통해 습득할 수 있는 포인트는 잡고 가자. CSS Module을 적용한 것에 만족할 것이 아니라 재사용성을 높인다거나 다른 기술 스택 대비 장점을 두드러지게 사용하는 등의 포인트가 필요하다.

하나의 함수가 너무 많은 일을 하지 않도록 신경 쓰자. 기본적인 거지만 당장은 잘 작동하는 데에 매몰되다 보면 놓치는 포인트이기도 하다.

2차 개발은 여분의 작업이 아닌 명확한 개선의 작업임을 다시금 생각하자. 중간에 개선의 방향성이 잘못되었다면 초기에 무얼 위해서 개선하려 했는지 다시금 돌아보고 방향성을 다시금 잡아가는 것 또한 필요하다고 생각한다.

### 참고자료

- https://bside.best/projects/detail/P240717090339
- https://gun0912.tistory.com/65
- [https://velog.io/@jeonghyegene/사이드프로젝트-포텐데이-EP-3.-DAY-2-부제-오프라인-모각프](https://velog.io/@jeonghyegene/%EC%82%AC%EC%9D%B4%EB%93%9C%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%ED%8F%AC%ED%85%90%EB%8D%B0%EC%9D%B4-EP-3.-DAY-2-%EB%B6%80%EC%A0%9C-%EC%98%A4%ED%94%84%EB%9D%BC%EC%9D%B8-%EB%AA%A8%EA%B0%81%ED%94%84)