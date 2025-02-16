### 활용 배경

프로덕트에 채널톡 플로팅 버튼이 추가됐다. 그런데 비슷한 위치에 최상단으로 스크롤하는 TOP 버튼이 존재하고 있었다. 두 버튼은 서로 다른 시점에 개발되어 크기와 위치가 미묘하게 달랐다. 자체 개발이었다면 [커스터마이징](https://developers.channel.io/reference/web-customization-kr)을 통해 별도의 커스텀 버튼을 붙였겠지만, 카페24 솔루션에 의존적인 서비스고 일단 커스텀이 되는지 불분명했기 때문에 기존 채널톡 버튼에 맞춰서 수정하기로 했다.

### JavaScript가 아닌 CSS로 해결하기

채널톡 버튼은 특정 기준에 따라 [PC 버튼은 56px, 모바일 웹 버튼은 44px로 사이즈가 자동 조절된다](https://docs.channel.io/help/ko/articles/94f34984#%EB%B2%84%ED%8A%BC-%EC%BB%A4%EC%8A%A4%ED%84%B0%EB%A7%88%EC%9D%B4%EC%A7%95). 개발자도구로 확인해 보니 이 기준이 viewport 기반이 아닌 것 같았다. 스타일 탭에서도 특정 미디어쿼리가 보이지 않았고, 아마도 [UA](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/User-Agent)를 파싱해서 처리하는 듯했다. 하지만 [UA는 축소되는 흐름](https://velog.io/@gonasooc/%EC%95%84%EC%A7%81%EB%8F%84-navigator.userAgent-User-Agent-%EC%B6%95%EC%86%8C%EC%99%80-User-Agent-Client-Hints)이기도 하고, CSS에서 처리하는 편이 별도의 부수 효과가 적을 거라는 판단이 들어 찾아보기로 했다.

### 명세에서 찾아보기

ChatGPT에게 물어보니 `pointer`와 `hover`를 추천하길래 조금 생소해서 명세에서 찾아보기로 했다. [Media Queries Level 4 명세](https://www.w3.org/TR/mediaqueries-4/)를 보면 viewport를 기준으로 흔히 사용하는 [`width`](https://www.w3.org/TR/mediaqueries-4/#width) 외에 [`height`](https://www.w3.org/TR/mediaqueries-4/#height)나 [`resolution`](https://www.w3.org/TR/mediaqueries-4/#resolution) 같은 익숙한 속성이 있고, [`color`](https://www.w3.org/TR/mediaqueries-4/#color) 같은 처음 보는 속성도 있었다. 물론 명세에 있는 모든 걸 사용할 필요는 없지만 [`pointer`](https://www.w3.org/TR/mediaqueries-4/#pointer)와 [`hover`](https://www.w3.org/TR/mediaqueries-4/#hover)는 이번에 적용하면 좋을 것 같았고, 실제로 이후에 종종 활용될 만한 여지가 있었다.

### Media Queries와 Media Queries Level 4

`pointer`와 `hover`는 Media Queries Level 4 명세에 새롭게 추가된 기능이다. Media Queries Level 4는 2017년 후보 권고안 단계의 지정이었기 때문에 타깃 브라우저를 확인할 필요는 있지만, 대부분의 최신 브라우저에서는 큰 문제 없이 사용할 수 있다. [이 글](https://wit.nts-corp.com/2023/03/29/6664)을 통해 자세히 확인할 수 있으며, 현재는 Media Queries Level 5까지 확장 및 보완되고 있긴 하다.

- Media Queries Level 3: 2012년에 W3C 권고안으로 지정, 거의 모든 브라우저에서 지원
- Media Queries Level 4: 2017년에 후보 권고안으로 지정, 일부 브라우저에서 지원 여부 확인 필요
- Media Queries Level 5: 2020년에 처음으로 공개되었으며 아직 초안 단계, 브라우저 지원률 낮음

### `pointer`와 `hover`

`pointer`는 마우스와 같은 포인팅 디바이스의 존재 여부와 정확도를 기준으로 판단한다. 포인팅 디바이스가 여러 개인 경우 기본이 되는 포인팅 디바이스(“primary” pointing device)의 특성을 반영한다고 한다.

- `none` - 포인팅 디바이스가 포함되어 있지 않음
- `coarse` - 스마트폰, 터치스크린 같이 포인팅은 가능하지만 정확도가 제한되어 있음
- `fine` - 마우스 같은 정확한 포인팅 장치가 포함됨

`hover`는 포인팅 디바이스로 해당 요소 위로 가져가는 상황을 판단한다. 이 또한 포인팅 디바이스가 여러 개인 경우 기본이 되는 포인팅 디바이스(“primary” pointing device)의 특성을 반영한다.

- `none` - 포인팅 디바이스가 hover를 할 수 없거나 포인팅 디바이스가 없음, 스마트폰이나 터치스크린도 여기에 포함
- `hover` - 포인팅 디바이스가 해당 요소 위에 명확히 hover를 할 수 있음

|  | [pointer: none](https://www.w3.org/TR/mediaqueries-4/#descdef-media-pointer) | [pointer: coarse](https://www.w3.org/TR/mediaqueries-4/#descdef-media-pointer) | [pointer: fine](https://www.w3.org/TR/mediaqueries-4/#descdef-media-pointer) |
| --- | --- | --- | --- |
| [hover: none](https://www.w3.org/TR/mediaqueries-4/#descdef-media-hover) | 키보드 전용 컨트롤, 순차적/공간적(D-패드) 포커스 내비게이션 | 스마트폰, 터치스크린 | 기본 스타일러스 디지타이저(Cintiq, Wacom 등) |
| [hover: hover](https://www.w3.org/TR/mediaqueries-4/#descdef-media-hover) |  | 닌텐도 Wii 컨트롤러, 키넥트 | 마우스, 터치패드, 고급 스타일러스 디지타이저(Surface, Samsung Note, Wacom Intuos Pro 등) |
|  |  |  |  |

### UA 대신 활용하기

우선 `hover`를 할 수 없는 상황에 모바일 버튼 크기(44px)를 적용했다.

```css
/* hover 불가능 & 포인팅 장치가 정확하지 않은 경우 */
@media (hover: none) and (pointer: coarse) {
    .app .fixed_btn {
        width: 44px;
    }
}
```

일부 안드로이드 버전에서 `hover`를 지원한다고 판단하는 경우가 있어 `pointer`가 `coarse`일 때 해당 CSS가 적용되도록 작업했다. 이렇게 적용하면 viewport `width`를 떠나서 `hover`를 할 수 없으면서 마우스처럼 정밀한 포인팅이 불가능한 대부분의 스마트폰이나 터치스크린 환경에 해당 스타일을 적용할 수 있다.

### 모바일에 `hover` 스타일 제외하기

데스크탑에 적용된 `hover` 스타일이 모바일에서 `focus`될 때 의도치 않게 적용되는 경우가 있다. viewport `width`를 기준으로 스타일이 제외되도록 해결하기도 했지만, `pointer`와 `hover`를 활용하면 좀 더 간단하게 처리할 수 있다.

```css
/* 데스크탑 버튼의 적용될 hover 스타일 */
@media (hover: hover) and (pointer: fine) {
    .app .desktop_button {
        background-color: red;
        color: #fff;
        font-weight: 600;
    }
}
```

`hover`가 가능한 상황이면서 동시에 `pointer`가 `fine`인 상황, 즉 정밀한 입력 장치(마우스 등)에서만 스타일을 적용하면 모바일에서 의도하지 않은 스타일을 제외할 수 있다.

### 참고자료

- [https://marshallku.com/dev/css로-기기-파악하기](https://marshallku.com/dev/css%EB%A1%9C-%EA%B8%B0%EA%B8%B0-%ED%8C%8C%EC%95%85%ED%95%98%EA%B8%B0)
- https://www.w3.org/TR/mediaqueries-4/#mf-interaction
- [https://velog.io/@qhflrnfl4324/미디어-쿼리를-이용한-크로스-브라우징-CSS](https://velog.io/@qhflrnfl4324/%EB%AF%B8%EB%94%94%EC%96%B4-%EC%BF%BC%EB%A6%AC%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%ED%81%AC%EB%A1%9C%EC%8A%A4-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A7%95-CSS)
- https://wit.nts-corp.com/2023/03/29/6664