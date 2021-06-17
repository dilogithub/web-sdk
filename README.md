# Web SDK Guide

# README

---

IE를 제외한 브라우저 환경에서 사용하시길 권장합니다

# 적용방법 (Example)

---

```html
<!-- Universal Module Definition -->
<html>
  <head>
    <!-- ... -->
    <script src="PATH_HERE/dilo-sdk.umd.min.js"/>
  </head>
  <body>
    <!-- ... -->
  </body>
</html>
```

# 광고 요청 샘플코드

---

```js
// DiloSDK → Global Variable

// ### 광고 요청전 설정 ###

// `productType`이 `dilo`가 아닌값을 이용할 경우
// 컴패니언이 노출될 Element를 설정해주어야합니다.
// companion: HTMLDivElement
DiloSDK.setCompanionSlot(companion);

// 컴패니언을 숨길수 있는 버튼설정 (optional)
// *이벤트를 설정하지 않으셔도 됩니다
// closeButton: HTMLElement
DiloSDK.setCloseButton(closeButton);

// Function: () => void
DiloSDK.onNoFill(Function);

// ### 이벤트 설정 ###

// 광고요청 후 응답된 광고가 1개 이상일 경우 호춣
// Function: () => void
DiloSDK.onAdReady(Function);

// 광고가 시작되었을경우 호출
// Function: (IAdInfo) => void
DiloSDK.onAdStart(Function);

// 광고가 시작중일때 진행률을 포함하여 호출
// Function: (IProgress) => void
DiloSDK.onTimeUpdate(Function);

// 광고가 중지되었을경우 호출
// Function: () => void
DiloSDK.onPause(Function);

// 광고가 재개되었을경우 호출
// Function: () => void
DiloSDK.onResume(Function);

// 스킵가능한 광고일경우 스킵가능한 시점에 호출
// Function: () => void
DiloSDK.onSkipEnabled(Function);

// 현재 재생중인 광고가 완료되었을경우 호출
// Function: () => void
DiloSDK.onAdCompleted(Function);

// 모든 광고가 재생완료되었을경우 호출
// Function: () => void
DiloSDK.onAllAdsCompleted(Function);

// 에러상황시 호출
// Function: (string) => void
DiloSDK.onError(Function);

// 컴패니언이 `closeButton`이벤트에 의해 사라졌을경우 호출
// Function: () => void
DiloSDK.onCompanionClosed(Function);

// ### 광고 요청 ###
// 같은 `epiCode`로 여러번 광고 요청할경우
// 최초 한번 광고는 정상처리되고 그 이후 요청은 onError를 통해서
// 'TOO_MANY_REQUEST'이벤트 발생
DiloSDK.requestAd({
  // DILO에서 받은 Bundle Id (DILO에 문의해주세요)
  bundleId: '${BUNDLE_ID}',

  // 에피소드 URL
  // 예) test.com/audio/episode.mp3 
  // URL일경우 http|https의 스키마는 빼고 광고요청하는것을 권장
  epiCode: '${EPISODE_URL}',

  // 광고요청한 에피소드가 속해있는 채널명
  // 추후 리포트에 반영되는 값
  // 채널명이 없을경우 리포트에 보여질 임의값 설정
  chName: '${CHANNEL_NAME}',

  // 광고요청한 에피소드의 타이틀
  // 추후 리포트에 반영되는 값
  // 에피소드명이 없을경우 리포트에 보여질 임의값 설정
  epName: '${EPISODE_NAME}',

  // 광고요청한 에피소드의 창작자 중복되지 않는 고유식별값
  // DILO가 아닌 구현하는 쪽에서 식별할수있는 값
  // 예) `user#0001`, `12391`, `user@dilo.co.kr` ...
  // 중복될 경우 같은 창작자로 인식하여 리포트 집계
  // *이메일등 개인정보가 포함된 값일경우 MD5, SHA1등 해시 암호를 이용하여 요청
  creatorIdentifier: '${CREATOR_ID}',

  // 광고요청한 에피소드의 창작자명
  // 추후 리포트에 반영되는 값
  // 창작자명이 없을경우 리포트에 보여질 임의값 설정
  creatorName: '${CREATOR_NAME}',

  // 광고시간 설정
  // RequestParam.FillType.SINGLE_ANY일 경우 null 설정
  drs: fillType === 'single_any' ? null
                                 : ${DURATION},

  // 광고타입 설정
  // 'dilo' -> 오디오만 나오는 광고
  // 'dilo_plus' -> 오디오 + 컴패니언 광고
  // 'dilo_plus_only' -> 오디오 + 컴패니언 광고 (컴패니언이 무조건 포함)
  productType: '${PRODUCT_TYPE}',

  // 광고 갯수 타입 설정. RequestParam.FillType 참조
  // 'single' -> 단일 광고 요청 drs에 맞는 길이의 광고요청
  // 'multi' -> 아래 drs값의 길이에 맞게 광고요청
  // 'single_any' -> drs와 상관없이 6초, 10초, 15초 랜덤의 단일 광고요청
  fillType: '${FILE_TYPE}',

  // 광고를 호출한 시점에서의 위치
  // 에피소드를 시작하기전에 호출할경우 'pre'
  // 에피소드 중간에 광고를 호출할경우 'mid'
  // 에피소드가 끝난후 호출할경우 'post'
  adPosition: '${AD_POSITION}'
});

// ### 요청된 광고 재생 ###
// 광고요청이 비동기로 진행되므로
// .onAdReady(Function) 이벤트에 아래 코드를 사용
// 예) DiloSDK.onAdReady(() => DiloSDK.start())
DiloSDK.start();

// 재생중인 광고 중지/재생
DiloSDK.playOrPause();

// 광고 중지(완료처리) 다시 재생불가
DiloSDK.stop();
```
