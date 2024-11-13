### **HTTP headers에 디바이스 정보 보내기**

내가 개발한 서비스는 서버에 API 요청을 보낼 때 headers에 많은 정보를 담아 보내는데, 그 중 디바이스 정보 또한 포함되어 있다. 일반적으로 웹에서는 디바이스 정보를 포함하진 않지만, 앱 클라이언트에서는 다양한 디바이스 정보를 포함하고 있었다. 이미 앱에서 사용하고 있는 API를 동일하게 사용해야 하는 경우도 있었기 때문에 통일성 있게 맞추는 게 낫다는 판단 및 논의가 있었다. 브라우저에서 사용자 정보를 파싱해야 했다. 

전역 객체에서 접근할 수 있는 `navigator`의 [`userAgentData`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/userAgentData) property나 [User-Agent Client Hints API](https://developer.mozilla.org/en-US/docs/Web/API/User-Agent_Client_Hints_API)(이하 UA-CH)와 아직 ‘Experimental’로 분류하기 때문에 [`userAgent`](https://developer.mozilla.org/ko/docs/Glossary/User_agent) property를 파싱하는 게 안전하다고 판단했다. 다만 `userAgent`의 경우 MS로부터 시작된 ~~똥 싸지르기~~ [브라우저 제작사들의 스푸핑](https://wormwlrm.github.io/2021/10/11/Why-User-Agent-string-is-so-complex.html)과 [호환성 이슈](https://guiyomi.tistory.com/150)로 지옥에서 온 것처럼 문자열이 복잡하기 때문에 UAParcer라는 라이브러리를 사용했다. 코드가 간소해진다는 [확실한 장점](https://docs.uaparser.dev/intro/why-ua-parser-js.html)이 있었고, 정규식을 통한 문자열의 재가공 정도이기 때문에 번들 사이즈도 크지 않았다. `UAParser`라는 생성자 객체를 호출해서 `getResult()`를 구조 분해 할당하는 방식을 취했다. 

```jsx
  const parser = new UAParser();
  const { browser, device } = parser.getResult();

  const config = {
    osName: 'Web',
    userCountry: country,
    appVersion: '1.3.4', // TODO: 라이브 배포 시 최신 버전으로 업데이트
    deviceManufacturer: device.vendor,
    deviceModel: device.model,
    userLocale: selectedLanguage, // 사용자가 선택한 언어 정보
    userTimeZone: userTimeZone,
    mediaLocation: locationData.location, // local 환경에선 seoul
    deviceType: device.type === undefined ? 'desktop' : device?.type, // desktop의 경우 parser의 반환값이 undefined라 별도의 string 부여
    browserName: browser.name,
    browserVersion: browser.version,
    duid: uuid,
  };
```

### **디바이스 모델에 `K`?**

처음 해당 이슈를 접한 건 서버 개발자 분의 확인 요청이었다. 앞서 설명한 것처럼 디바이스 정보를 담아서 API 요청을 보냈는데, 디바이스 모델 정보에 `K`’라는 값이 담긴다는 것이었다. 내부 CMS를 통해 데이터를 확인한 결과, 특정 기간만이 아니라 웹 서비스를 시작한 이후로 계속 담겨 있었던 것으로 보였다.

### **User-Agent(이하 UA) 축소**

결론부터 말하면 ~~내똥내치~~ [UA 축소(User-Agent reduction)](https://developers.google.com/privacy-sandbox/protections/user-agent)의 일환이었다. UA 축소와 UA-CH로의 대체는 두 가지의 목적을 두고 있다고 한다.

- UA는 꽤 ~~난잡한~~ 자세한 정보를 포함하고 있기 때문에 사용자를 식별할 수 있어 축소를 통해 사용자의 개인 정보 보호를 개선
- UA는 앞서 말한 [제작사들의 스푸핑](https://wormwlrm.github.io/2021/10/11/Why-User-Agent-string-is-so-complex.html)과 [호환성 이슈](https://guiyomi.tistory.com/150)로 인해 신뢰할 수 없는 데이터까지 파싱하게 되어 오류가 발생하기 쉽기 때문에 UA-CH를 통해 더 쉽게 구조화되어 있고 신뢰할 수 있는 데이터 제공

`K` 문자열 같은 경우에는 Chrome 110(2023년 2월)부터 안드로이드 모델에 점진적으로 도입된 문자열로, 구글의 [user-agent reduction과 관련된 안내](https://developers.google.com/privacy-sandbox/blog/user-agent-reduction-android-model-and-version)에서 해당 내용을 확인할 수 있다.

- Before: UA에 안드로이드 버전 및 기기 모델 포함
    
    ```
    Mozilla/5.0 (Linux; Android 13; Pixel 7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.0.0 Mobile Safari/537.36
    ```
    
- After: 안드로이드 버전 및 기기 모델 고정으로 UA 축소
    
    ```
    Mozilla/5.0 (Linux; Android 10; K) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.0.0 Mobile Safari/537.36
    ```
    

해외에서도 [‘The mysterious model K’라는 아티클](https://mobiforge.com/research-analysis/the-mysterious-model-k)을 통해 해당 내용을 언급하기도 한다.

```
Mozilla/5.0(Linux; Android 10; K) AppleWebKit/537.36(KHTML, like Gecko) Chrome/<majorVersion>.0.0.0 Mobile Safari/537.36
```

말 그대로 UA의 특정 값들을 고정하기 때문에 프리징이라는 표현을 사용한다. Chrome 83에서 User-Agent Client hints가 실험적으로 도입되면서 프리징이 점진적으로 적용되었다. UA뿐만 아니라 `navigator.appVersion`, `navigator.platform`, `navigator.productSub`, `navigator.vendor` 같은 값들이 고정되고, 운영체제에 관련된 정보도 프리징 대상에 포함된다. 실제로 그 무렵 포스팅된 [프리징 대비와 관련된 아티클](https://d2.naver.com/helloworld/6532276)이 꽤나 눈에 띄었다. 

### **그럼 UA는 못 쓰나요?**

UA 문자열 자체는 축소되었지만 여전히 접근할 수 있다. 하지만 점차 제한적일 가능성이 높다. 아직 UA를 완전히 배제할 필요는 없지만, 프라이버시 향상과 더 정확한 사용자 정보를 얻기 위해서는 UA-CH로의 전환을 고려할 필요성은 있는 것 같다.

UA 정보가 필요 없는 경우엔 상관없지만, 사용자의 브라우저 또는 디바이스에 최적화된 컨텐츠를 제공하려고 할 때 영향을 줄 수 있을 것 같다. 물론 우선적으로 웹 표준을 준수하는 것이 중요하지만, 작업을 하다 보면 특정 케이스를 다뤄야 할 때가 생기기 마련이다.

아이러니하게도 UA를 망쳐놓은 브라우저 제작사들이 UA의 단점을 극복하고자 UA-CH를 도입했다. UA-CH는 필요한 사용자 정보를 더 명시적이고 안전하게 요청할 수 있도록 설계되었기 때문에 이를 통해 디바이스 종류나 브라우저 버전, 플랫폼 등의 구체적인 정보를 구조화된 형태로 얻는 게 가능하다.

다만 모든 사용자가 최신 브라우저를 사용하는 것은 아니기 때문에 UA-CH를 지원하지 않는 브라우저를 위해 UA 파싱을 폴백으로 유지하면서 점진적으로 UA-CH로 전환하는 것도 방법이 될 수 있겠다.

### 참고자료

- https://developer.mozilla.org/ko/docs/Glossary/User_agent
- https://developer.mozilla.org/en-US/docs/Web/API/User-Agent_Client_Hints_API
- https://wormwlrm.github.io/2021/10/11/Why-User-Agent-string-is-so-complex.html
- https://guiyomi.tistory.com/150
- https://docs.uaparser.dev/intro/why-ua-parser-js.html
- https://developers.google.com/privacy-sandbox/protections/user-agent
- https://developers.google.com/privacy-sandbox/blog/user-agent-reduction-android-model-and-version
- https://mobiforge.com/research-analysis/the-mysterious-model-k
- https://d2.naver.com/helloworld/6532276
- https://dc2348.tistory.com/59