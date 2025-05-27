### 처음 만난 HLS

재작년 회사에 입사하자마자 다루게 된 것이 HLS였다. 그전까지는 mp4 같은 포맷으로 서빙된 영상만 video 태그로 다뤘기 때문에 m3u8을 어떻게 다뤄야 하는지 명확히 몰랐고, 기능 구현에 정신이 없었다([굳이 video.js를 쓸 필요가 없었는데 개고생했던..](https://velog.io/@gonasooc/%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%ED%9A%8C%EA%B3%A0-%EB%AE%A4%EB%B9%97%EB%9D%BC%EC%9D%B4%EB%B8%8C-1%EC%B0%A8-%ED%9A%8C%EA%B3%A0#videojs)). 이번에 사내 프로토타입에서 m3u8을 다시 다루게 되면서, ts가 TypeScript가 아니라는 사실(…)에서 끝날 게 아니라 조금 더 깊이 있게 살펴보고 싶어졌다.

### 적응형 비트레이트 스트리밍의 핵심, m3u8과 HLS

프론트 개발자로서 브라우저에서 시작된 연속된 의문과 학습 과정을 찬찬히 기록해 보려 한다. 서버에 요청을 보내면 받아오는 것이 주로 m3u8이다. m3u8은 HTTP 라이브 스트리밍(HLS)에서 사용하는 확장자 파일이다. HLS는 말 그대로 HTTP 프로토콜을 기반으로 하는 적응형 비트레이트 스트리밍(Adaptive bitrate streaming) 프로토콜인데, 간단히 설명하면 네트워크 상황에 따라 자동으로 화질을 조정하는 스트리밍 기술이다.

![마스터 플레이리스트와 미디어 플레이리스트, 미디어 세그먼트의 구성([원본 출처](https://developer.apple.com/library/archive/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html))](attachment:83b9a5f8-33dc-469b-913a-7f7362a93e8a:stream_playlists_2x.png)

마스터 플레이리스트와 미디어 플레이리스트, 미디어 세그먼트의 구성([원본 출처](https://developer.apple.com/library/archive/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html))

핵심 동작 방식을 요약하면 다음과 같다.

1. video 태그에 마스터 플레이리스트(m3u8)를 src로 제공하거나, video.js나 hls.js를 통해 m3u8을 로드한다.
2. 상황에 따라 파싱 주체는 조금 다른데, Safari는 m3u8을 네이티브로 파싱, 그 외에는 라이브러리를 통해 파싱한다.
    - HLS는 애플이 개발했고 Safari만이 네이티브로 지원하기 때문이다.
3. 첫 번째 m3u8(마스터 플레이리스트)을 파싱해서 선언된 각각의 다른 미디어 플레이리스트(m3u8)를 파싱한다.
4. 미디어 플레이리스트의 m3u8에서 세그먼트(ts 파일)와 세그먼트 재생 정보를 참고하여 재생한다.
    - 별도의 미디어 플레이리스트 없이 구성한다면 하나의 플레이리스트에 세그먼트만 선언하면 충분하다.

### 마스터 플레이리스트(m3u8)을 까보자

개념만으로는 와닿지 않아서 파일을 까보기로 했다. [hls.js demo 페이지](https://hlsjs.video-dev.org/demo/)에서 이미 구성된 m3u8 파일을 다운로드할 수 있다. 다양한 형식의 파일을 제공하므로 레퍼런스 체크용으로도 유용하다. [다운로드](https://test-streams.mux.dev/x36xhzz/x36xhzz.m3u8)

다운로드한 파일을 텍스트 에디터로 열어보면 다음과 같다.

```
#EXTM3U
#EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=2149280,CODECS="mp4a.40.2,avc1.64001f",RESOLUTION=1280x720,NAME="720"
url_0/193039199_mp4_h264_aac_hd_7.m3u8
#EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=246440,CODECS="mp4a.40.5,avc1.42000d",RESOLUTION=320x184,NAME="240"
url_2/193039199_mp4_h264_aac_ld_7.m3u8
#EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=460560,CODECS="mp4a.40.5,avc1.420016",RESOLUTION=512x288,NAME="380"
url_4/193039199_mp4_h264_aac_7.m3u8
#EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=836280,CODECS="mp4a.40.2,avc1.64001f",RESOLUTION=848x480,NAME="480"
url_6/193039199_mp4_h264_aac_hq_7.m3u8
#EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=6221600,CODECS="mp4a.40.2,avc1.640028",RESOLUTION=1920x1080,NAME="1080"
url_8/193039199_mp4_h264_aac_fhd_7.m3u8
```

처음 까봤다면 생소한 뭔 듣보(…) 태그들이 나열되어 있다.

첫 줄의 `#EXTM3U`는 필수 태그로 이 파일이 m3u8 임을 알리는 필수 헤더다. 파일의 맨 첫 줄에 위치해야 파서가 인식하고 파싱을 시작한다. Extended M3U라는 의미로 기본적으로 m3u의 확장이며, U는 UTF-8 인코딩을 명시한다.

`EXT-X-STREAM-INF` 는 각 미디어 플레이리스트의 메타데이터를 정의한다. 즉 해상도나 비트레이트, 인코딩된 스트림 정보를 제공한다. `PROGRAM-ID=1` 은 고유 식별자인데 크게 사용되지 않아 현재는 deprecated 상태이고, `BANDWIDTH=2149280` 는 말 그대로 대역폭을 설정하고 현재 네트워크의 가용 대역폭을 추정해서 `BANDWIDTH` 값과 가장 적합한 스트림을 선택하게 된다. 

`CODECS="mp4a.40.2,avc1.64001f"` 는 각각의 코덱으로 `mp4a.40.2` 는 AAC-LC 오디오 코덱, `avc1.64001f` 는 H.264 비디오 코덱 (High Profile, Level 3.1)을 의미한다. FFmpeg을 통해 인코딩할 때 어떤 인코딩 옵션을 주느냐에 따라 결정된다. 

`RESOLUTION=1280x720` 는 비디오의 해상도를 나타내며, ts 파일마다 해상도가 다르니 네트워크 대역폭과 함께 플레이어의 해상도 설정에 따라 다른 해상도를 선택하는 데에 사용된다. `NAME="720"`은 플레이어 UI에 표시할 스트림 이름이다. 사용자가 수동으로 화질을 선택할 수 있는 옵션이 있다면 이 `NAME` 속성이 표시된다.

이 마스터 플레이리스트엔 자막이 없지만, 자막이 있는 경우 `#EXT-X-MEDIA`로 선언할 수 있다. 더 많은 태그 정보는 [Apple Developer 공식 문서](https://developer.apple.com/documentation/http-live-streaming/live-playlist-sliding-window-construction)나 [IETF 표준 문서](https://datatracker.ietf.org/doc/html/draft-pantos-hls-rfc8216bis-17)에서 추가로 확인 가능하다. 

### 미디어 플레이리스트(m3u8)를 까보자

마스터 플레이리스트 중 `#NAME=720`의 `193039199_mp4_h264_aac_hd_7.m3u8` 를 까봤다. 

```
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-PLAYLIST-TYPE:VOD
#EXT-X-TARGETDURATION:11
#EXTINF:10.000,
url_462/193039199_mp4_h264_aac_hd_7.ts
#EXTINF:10.000,
url_463/193039199_mp4_h264_aac_hd_7.ts
#EXTINF:10.000,
...
(중략)
...
url_522/193039199_mp4_h264_aac_hd_7.ts
#EXTINF:10.000,
url_523/193039199_mp4_h264_aac_hd_7.ts
#EXTINF:10.000,
url_524/193039199_mp4_h264_aac_hd_7.ts
#EXTINF:4.584,
url_525/193039199_mp4_h264_aac_hd_7.ts
#EXT-X-ENDLIST
```

마스터 플레이리스트와 동일하게 `#EXTM3U` 로 시작하고, `#EXT-X-VERSION:3` 는 HLS 프로토콜 버전을 명시한다. 세그먼트 관련 기능이 없는 플레이리스트에는 명시하지 않지만, 뭐 명시해도 문제는 없다.

- 버전 3: 가장 보편적으로 쓰이는 버전
- 버전 4~7: DRM, 광고, 자막, 저지연 등 고급 기능이 필요할 때 사용
- 버전 8 이상: Low-Latency HLS(LL-HLS)를 구현 시 사용

일반적으로 버전 3에서 5 정도가 많이 사용된다.

`#EXT-X-PLAYLIST-TYPE:VOD` 은 플레이리스트 타입을 정의한다. VOD는 정적으로 고정된 플레이리스트로 전체 구간의 탐색이 가능하고, 반드시 `#EXT-X-ENDLIST`를 포함해야 한다. EVENT는 시간에 따라 점진적으로 확장되는 플레이리스트로 라이브 중계 등에 사용되고 과거 시점은 탐색 가능하다. [명세](https://datatracker.ietf.org/doc/html/draft-pantos-hls-rfc8216bis-17#section-4.4.3.5)에 따르면 옵셔널한 태그로 해당 태그가 없으면 라이브 스트림으로 간주하지만, VOD로 선언하고 `#EXT-X-ENDLIST`가 없으면 명세 위반(non-conformant)이 된다.

`#EXT-X-TARGETDURATION:11` 는 모든 세그먼트 중 가장 긴 지속 시간을 초 단위의 정수로 정의한다. 이는 클라이언트(플레이어)의 버퍼링 전략 수립에 중요한 역할을 한다. `#EXTINF:10.000` 처럼 각 세그먼트를 소수점으로 정의하는데, `#EXT-X-TARGETDURATION` 가 `#EXTINF` 보다 작으면 RFC 위반으로 간주된다. RFC 위반인 경우 대부분의 플레이어 단위에서 재생은 시도하지만 콘솔 등으로 경고를 표시한다.

마지막 라인에 드디어 상대 경로의 세그먼트 URL이 나온다. 순차적으로 재생되어야 하는 세그먼트들이 나열되어 있고, `#EXT-X-ENDLIST`는 더 이상의 세그먼트가 없음을 의미한다. 이 세그먼트 URL을 순차적으로 재생하는 것이 우리가 보는 스트리밍 영상이라고 할 수 있다.

다시 한번 정리하면, 마스터 플레이리스트에서 파싱된 스트림 정보를 기반으로 브라우저가 현재 네트워크 상황을 포함해 화면 크기, 기기 성능을 고려해 초기 품질을 선택한다. 적응형 비트레이트 스트리밍이기 때문에 실시간으로 품질이 조정되는데, 예컨대 네트워크 스루풋이 다운로드되는 세그먼트의 비트레이트보다 더 높다고 클라이언트가 판단하면 더 높은 비트레이트 세그먼트를 요청한다. 이렇게 HLS는 네트워크 상황에 따라 자동으로 품질을 조정하면서 원활한 스트리밍을 제공한다.

```
1. GET x36xhzz.m3u8 (마스터 플레이리스트)
2. GET url_0/193039199_mp4_h264_aac_hd_7.m3u8 (720p 세그먼트 목록)
3. GET segment0.ts (첫 번째 비디오 세그먼트)
4. GET segment1.ts (두 번째 비디오 세그먼트)
5. ... (순차적으로 세그먼트 다운로드)
```

### 클라이언트에 m3u8이 도착하기 이전의 과정

사실 프론트 입장에서는 m3u8에 대한 개념만 알아도 구현하는 데에 무리가 없다. 근데 기술 구현을 하다 보니 자연스럽게 궁금한 것들이 생겼다. ‘영상을 촬영한다’라는 최초의 행위 이후에 어떻게 처리되는 걸까. [네이버 D2 포스팅](https://d2.naver.com/helloworld/7122)을 참고할 수 있는데, 이게 생각보다 다양한 개념이 결합되어 있어 복잡하지만 흥미로웠다.

![촬영부터 클라이언트까지의 과정([원본 출처](https://d2.naver.com/helloworld/7122))](attachment:05005780-dec9-4b0d-817a-bdd0dbc933d6:helloworld-7122-2.png)

촬영부터 클라이언트까지의 과정([원본 출처](https://d2.naver.com/helloworld/7122))

여기서는 개념을 자세히 풀지 않고 이해한 부분만 간단하게 요약하자면,

1. 마이크와 카메라로 캡처, 즉 촬영하고, 촬영한 데이터를 효율적으로 전달하기 위해 압축과 인코딩 작업을 거친다. 
2. 압축과 인코딩 작업을 거친 영상 데이터를 RTSP(Real-Time Streaming Protocol)/RTP(Real-time Transport Protocol), RTMP(Real-Time Messaging Protocol) 등의 프로토콜을 지원하는 OBS를 통해 전송 규격에 맞게 서버로 전송한다.
3. 서버는 해당 프로토콜로 전송된 동영상을 읽어서 FFmpeg과 같은 오픈소스로 HLS 프로토콜의 형식으로 변환한다. 이 변환 과정에서 리소스를 많이 사용하기 때문에 별도의 인코딩 서버나 캡처보드를 통해 진행할 수도 있다.
4. 변환 과정에서 ts 세그먼트는 FFmpeg에서 설정된 인코딩 옵션에 따라 구성된다. 하나의 영상이라도 품질에 따라 여러 세그먼트를 동시에 생성하게 된다.
5. 해당 세그먼트를 S3 같은 저장소에 담아서 서빙할 수도 있지만, 일반적으로 빠른 제공을 위해 CDN을 통해 클라이언트에 제공하게 된다.

자세한 과정은 생략했기에 간단하게 설명됐지만, 설명의 내부 과정에서 많은 작업들이 일어난다. 해당 프로토콜이나 설명하지 못한 작업은 추후 추가로 학습해 보려 한다.

### 라이브 스트림에서의 실시간 폴링

앞서 언급했듯 라이브 스트림의 경우 VOD와 달리 `#EXT-X-ENDLIST` 태그가 없다. 그렇다면 클라이언트는 별도의 폴링 설정을 하지 않는데도 어떻게 새로운 세그먼트가 추가되는 걸 알 수 있을까?

라이브 스트림에서 플레이어(hls.js, video.js 등)는 다음과 같은 방식으로 작동한다:

1. 클라이언트가 첫 m3u8 파일로 현재까지 생성된 세그먼트 목록을 받는다.
2. `#EXT-X-TARGETDURATION` 값을 기준으로 플레이어가 자동으로 플레이리스트를 다시 요청한다(일반적으로 `#EXT-X-TARGETDURATION`의 1/2 또는 1/3 간격으로 폴링).
3. 폴링 이후 새로운 세그먼트가 추가되었다면 해당 세그먼트를 다운로드하여 재생 큐에 추가한다.
4. 오래된 세그먼트는 플레이리스트에서 제거되어 메모리 사용량을 관리한다.

```
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-TARGETDURATION:10
#EXT-X-MEDIA-SEQUENCE:100
#EXTINF:10.0,
segment100.ts
#EXTINF:10.0,
segment101.ts
#EXTINF:10.0,
segment102.ts
```

`#EXT-X-MEDIA-SEQUENCE`는 첫 번째 세그먼트의 시퀀스 번호를 나타낸다. 라이브 스트림에서는 오래된 세그먼트가 계속 제거되므로 현재 플레이리스트의 시작점을 알려주는 중요한 역할을 한다.

### Safari 외 브라우저의 HLS 처리 방식

Safari 외에는 직접 텍스트 기반의 스트리밍 목록 파일인 m3u8을 파싱할 수 없다. hls.js 같은 라이브러리는 아래와 같이 사용한다.

```jsx
  var video = document.getElementById('video');
  var videoSrc = 'https://test-streams.mux.dev/x36xhzz/x36xhzz.m3u8';
  if (Hls.isSupported()) {
    var hls = new Hls();
    hls.loadSource(videoSrc);
    hls.attachMedia(video);
  }
```

Safari 외의 브라우저에서 라이브 스트림을 제어하기 위해서는 Media Source Extensions(MSE)를 사용하는데, 실시간 재생을 위한 API이다. 2013년 Google과 Microsoft 주도로 표준화된 기술로, 실시간 스트리밍이나 적응형 비트레이트 전환이 불가능한 video 태그의 한계 때문에 등장했다. 세그먼트는 fMP4(fragmented MP4)를 사용한다. 요즘엔 HLS도 처음부터 m3u8의 ts 대신 fMP4를 사용하는 경우가 많은데, 이를 HLS with CMAF라고 부른다. 이런 경우 당연히 별도의 변환 없이 MSE에 바로 넣을 수 있어 효율적이다. MSE를 통해 fMP4를 넣는 과정은 다음과 같다.

```jsx
const mediaSource = new MediaSource();
video.src = URL.createObjectURL(mediaSource);

mediaSource.addEventListener('sourceopen', () => {
  const sourceBuffer = mediaSource.addSourceBuffer('video/mp4; codecs="avc1.42E01E, mp4a.40.2"');
  fetch('segment1.fmp4')
    .then(res => res.arrayBuffer())
    .then(data => sourceBuffer.appendBuffer(data));
});
```

hls.js가 해당 m3u8을 파싱해서 ts 세그먼트를 받아온다. ts 같은 비표준 세그먼트는 MSE에 넣을 수 없기 때문에 fMP4로의 변환 과정을 거쳐서 MSE를 통해 재생한다.

### hls.js는 어떻게 Safari를 구분할까?

Safari는 네이티브로 HLS 가능하다고 했다. 그 말은 hls.js를 적용한 컴포넌트라도 Safari에서는 해당 라이브러리를 쓸 필요가 없다. 그럼 hls.js는 어떻게 브라우저를 판별할까. 정확히 말하면 브라우저를 판별하는 게 아니라 MSE 지원 여부를 판별한다. 앞서 예시로 사용되기도 했던 `isSupported`의 로직을 확인하기 위해 라이브러리 내부의 `is-supported.ts` 까보면,

```tsx
import { mimeTypeForCodec } from './utils/codecs';
import { getMediaSource } from './utils/mediasource-helper';
import type { ExtendedSourceBuffer } from './types/buffer';

function getSourceBuffer(): typeof self.SourceBuffer {
  return self.SourceBuffer || (self as any).WebKitSourceBuffer;
}

export function isMSESupported(): boolean {
  const mediaSource = getMediaSource();
  if (!mediaSource) {
    return false;
  }

  // if SourceBuffer is exposed ensure its API is valid
  // Older browsers do not expose SourceBuffer globally so checking SourceBuffer.prototype is impossible
  const sourceBuffer = getSourceBuffer();
  return (
    !sourceBuffer ||
    (sourceBuffer.prototype &&
      typeof sourceBuffer.prototype.appendBuffer === 'function' &&
      typeof sourceBuffer.prototype.remove === 'function')
  );
}

export function isSupported(): boolean {
  if (!isMSESupported()) {
    return false;
  }

  const mediaSource = getMediaSource();
  return (
    typeof mediaSource?.isTypeSupported === 'function' &&
    (['avc1.42E01E,mp4a.40.2', 'av01.0.01M.08', 'vp09.00.50.08'].some(
      (codecsForVideoContainer) =>
        mediaSource.isTypeSupported(
          mimeTypeForCodec(codecsForVideoContainer, 'video'),
        ),
    ) ||
      ['mp4a.40.2', 'fLaC'].some((codecForAudioContainer) =>
        mediaSource.isTypeSupported(
          mimeTypeForCodec(codecForAudioContainer, 'audio'),
        ),
      ))
  );
}

export function changeTypeSupported(): boolean {
  const sourceBuffer = getSourceBuffer();
  return (
    typeof (sourceBuffer?.prototype as ExtendedSourceBuffer)?.changeType ===
    'function'
  );
}

```

`getMediaSource` 는 `mediasource-helper.ts`에 선언되어 있다.

```tsx
export function getMediaSource(
  preferManagedMediaSource = true,
): typeof MediaSource | undefined {
  if (typeof self === 'undefined') return undefined;
  const mms =
    (preferManagedMediaSource || !self.MediaSource) &&
    ((self as any).ManagedMediaSource as undefined | typeof MediaSource);
  return (
    mms ||
    self.MediaSource ||
    ((self as any).WebKitMediaSource as typeof MediaSource)
  );
}
```

대략적으로 `ManagedMediaSource`, `self.MediaSource`, `self.WebKitMediaSource`를 통해 MSE 지원 여부를 체크하고, `getSourceBuffer`를 통해 `SourceBuffer` 객체를 가져온다. 이는 브라우저가 MSE를 지원할 때 내부적으로 사용하는 객체로, 이 또한 MSE 지원 여부를 확인하는 용도다. `appendBuffer`와 `remove` 지원 여부를 다시 한번 체크하면서 제대로 구현되어 있는지 추가적으로 확인한다. MIME 타입 지원까지 확인하는데, 결국 `isSupported`가 `true`일 때 hls.js를 실행하게 된다. 이 과정에서 Safari는 `isSupported`가 `false`를 리턴하면서 네이티브로 실행하게 된다.

처음 hls.js를 초기화하는 시점에 `canPlayType()`을 통해 MSE가 아닌 네이티브로 실행하는 것 또한 가능하다. [README](https://github.com/video-dev/hls.js?tab=readme-ov-file#alternative-setup)를 통해 권장하는 방식이기도 하다.

```jsx
  var video = document.getElementById('video');
  var videoSrc = 'https://test-streams.mux.dev/x36xhzz/x36xhzz.m3u8';
  if (Hls.isSupported()) {
    var hls = new Hls();
    hls.loadSource(videoSrc);
    hls.attachMedia(video);
  }
  // HLS.js is not supported on platforms that do not have Media Source
  // Extensions (MSE) enabled.
  //
  // When the browser has built-in HLS support (check using `canPlayType`),
  // we can provide an HLS manifest (i.e. .m3u8 URL) directly to the video
  // element through the `src` property. This is using the built-in support
  // of the plain video element, without using HLS.js.
  else if (video.canPlayType('application/vnd.apple.mpegurl')) {
    video.src = videoSrc;
  }
```

### 전체 과정을 훑어보며

예전에는 단순히 video 태그에 URL만 넣으면 되는 줄 알았지만, 실제로는 꽤나 복잡한 프로토콜과 다양한 기술들이 조합된 결과물이라는 것을 알게 되었다. WebRTC를 활용한 더 낮은 지연시간의 스트리밍이나 AV1 같은 차세대 코덱도 한번 살펴보고 싶다. 그에 앞서, 여기서 다루지 못했던 내용들을 좀 더 톧아보고 싶은 마음이 있다.

세상에 별도의 과정이나 이유 없이 으레 당연한 것은 없다. 비개발자 입장이었을 때는 이미지나 동영상이 그저 카메라의 산출물이나 컨텐츠의 결과물로만 느껴졌지만, 하나의 포맷으로 컴퓨터에 존재하는 파일이 꽤 여러 과정을 거친 아웃풋이라는 걸 점차 느끼게 된다. OSI 7 레이어만 접해봐도 우리가 주로 접하는 애플리케이션 레이어 밑단에 얼마나 많은 과정을 거치는지 알게 되듯이, 컴퓨터 공학의 매력은 수면 아래 분주히 움직이는 백조의 발처럼 보이지 않는 치열함에 있다.

### 참고 자료

- https://d2.naver.com/helloworld/7122
- [https://medium.com/naver-cloud-platform/미디어-기술-이해-6단계로-알아보는-라이브-생방송-송출-원리-86a5137a3655](https://medium.com/naver-cloud-platform/%EB%AF%B8%EB%94%94%EC%96%B4-%EA%B8%B0%EC%88%A0-%EC%9D%B4%ED%95%B4-6%EB%8B%A8%EA%B3%84%EB%A1%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-%EB%9D%BC%EC%9D%B4%EB%B8%8C-%EC%83%9D%EB%B0%A9%EC%86%A1-%EC%86%A1%EC%B6%9C-%EC%9B%90%EB%A6%AC-86a5137a3655)
- https://developer.apple.com/library/archive/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html
- https://datatracker.ietf.org/doc/html/draft-pantos-hls-rfc8216bis-17
- https://ddicreator.com/92
- [https://jee00609.github.io/live stream/Live-Stream/](https://jee00609.github.io/live%20stream/Live-Stream/)
- [https://ko.wikipedia.org/wiki/적응_비트레이트_스트리밍](https://ko.wikipedia.org/wiki/%EC%A0%81%EC%9D%91_%EB%B9%84%ED%8A%B8%EB%A0%88%EC%9D%B4%ED%8A%B8_%EC%8A%A4%ED%8A%B8%EB%A6%AC%EB%B0%8D)
- https://duck-blog.vercel.app/blog/web/understand-hls