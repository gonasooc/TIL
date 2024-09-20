### Sentry가 뱉은 로그

최근에 회사에서 작업을 맡긴 외부 SI 업체로부터 프로젝트를 인계받았는데 Next.js 환경이었다. 대략적인 코드 파악을 하고 빌드하는 데 처음 보는 로그가 확인되었다.

```jsx
[@sentry/nextjs - Node.js] Info: Sending error and performance telemetry data to Sentry. To disable telemetry, set `options.telemetry` to `false`.
```

말인즉슨 Sentry로 telemetry 정보도 보내게 되니 해당 기능을 비활성화하고 싶으면 옵션에서 별도로 설정하라는 의미였다.

### telemetry가 뭐지?

[Next.js 공식 문서](https://nextjs.org/telemetry)에 telemetry에 대한 설명이 있다. 민감 정보를 제외한 익명의 데이터를 수집한다고 하고 수집 리스트는 아래와 같다.

- `next build` 같은 커맨드 명령어 호출 정보
- 사용 중인 Next.js의 버전
- 일반적인 머신 정보(CPU 수, OS 정보 등)
- 프로젝트 내 Next.js 플러그인 여부
- 빌드 시 페이지 수 같은 애플리케이션의 사이즈

`NEXT_TELEMETRY_DEBUG=1` 같은 환경변수 설정으로 수집 내용을 자세히 확인할 수 있고, 수집된 데이터는 개인 식별이 불가능하다고 한다.

### telemetry 활성화 여부 확인

프로젝트 루트에서 아래 명령어를 통해 활성화 여부를 확인할 수 있다.

```
npx next telemetry status
```

확인해 보니 활성화되어 있었다(사용하는 패키지 매니저에 따라 다르게 확인 e.g., `yarn next telemetry status`). 왜 활성화되어 있는 거지?

### 옵트인 없이 활성화?

회사 프로젝트와 무관하게 별도의 Next.js 환경을 구축해 보았다. 기존 프로젝트의 Next.js는 14.1.4, 첫 테스트 환경은 14.2.8이었는데 환경 구축 후 telemetry 활성화 여부를 확인해 봤을 때 비활성화였다. 마이너 업데이트 사이에 변경점이 있나 싶어서 동일한 14.2.8로 구성하고 확인해 봐도 비활성화였다.

[Next.js의 릴리즈 정보](https://github.com/vercel/next.js/releases)에서 telemetry로 검색 후 체크해봐도 별도로 명시되어 있진 않은 거 같았다.

### Next.js의 telemetry 비활성화

개인 식별이 안 되고 민감 정보는 아니라지만 어쨌든 사내 머신 정보나 프로젝트와 관련된 정보이기도 하고, 어쨌든 빌드 시에 관련 로그가 계속 뜨는 부분도 있기 때문에 비활성화 하기로 했다. 옵트아웃의 방법은 동일하게 프로젝트 루트에서 아래 명령어로 설정 가능하다.

```
npx next telemetry disable
```

### Sentry 옵션에서의 비활성화

다만 Next.js 내부에서 disable 설정을 했다 하더라도 아마 Sentry 내부에서는 별도로 감지할 수 없는 건지 동일한 로그가 나오고 있었다. 권장하는 방식대로 Sentry 옵션에서도 보내지 않도록 추가해 두었다.

```jsx
export default withSentryConfig(nextConfig, {
  org: "vlending-developer",
  project: "music-distribution-frontend",
  silent: false,
  telemetry: false,
});
```

### telemetry와 관련된 이슈

추측이지만 아마 초기엔 옵트인 없이 활성화됐던 건 아닐까. 물론 현재는 비활성화가 기본값이라 특별한 논의가 없지만 생각보다 관련된 논의나 피드백이 꾸준히 있어왔던 거 같다. Docker 환경에서 비활성화 설정이 다소 까다로운 부분 또한 있었던 걸 감안하면 수집의 목적은 이해하나 사용자의 명시적 동의를 받는 옵트인 방식을 취했어야 하지 않나 싶긴 하다. [따봉을 54개나 받은 형님의 코멘트](https://github.com/vercel/next.js/issues/8851#issuecomment-591710955)를 보면 단호하고 담백하게 문제점을 명시하고 있다.

### 참고자료

- https://nextjs.org/telemetry
- https://www.netlify.com/blog/2020/12/15/how-to-turn-off-telemetry-in-next.js/
- https://github.com/vercel/next.js/issues/59686
- https://github.com/vercel/next.js/issues/8851
- https://github.com/vercel/next.js/releases