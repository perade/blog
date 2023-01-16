---
title: "iOS에서 웹 대응 시 알아두면 좋은 몇 가지"
path: "ios-web-browser-tips"
date: 2020-10-29 00:00:00
published: true
tags: ['JavaScript', 'Web API', 'HTML']
canonical_url: false
hasImage: false
description: "웹 개발 시 iOS를 대응하는 도중 맞닥뜨릴 수 있는 몇 가지 상황에 대해 간단히 알아본다."
---

최근 회사 서비스의 특정 기능을 구현하면서 iOS를 대응하느라 애를 먹은 경험이 있다. 대체로 PC나 안드로이드에서의 브라우저에선 문제 없이 동작하는데 iOS에선 의도한 대로 동작하지 않는 경우였다. 향후에도 비슷한 상황을 맞닥뜨릴수 있기에 해당 이슈들을 해결하거나 우회한 방법을 기록으로 남겨놓고자 한다.

***

# getUserMedia 지원

- ***TL;DR*** 사파리 외 브라우저에선 getUserMedia를 지원하지 않음
- iOS에 들어가는 타사 브라우저들은 iOS WebKit 엔진을 사용하여 구현해야 함
- 애플이 해당 엔진에서 저 기능을 범용적으로 열어놓지 않고, 사파리에서만 허용해놓음
    
#### 참고
[Chrome and Firefox are not able to access iPhone Camera](https://stackoverflow.com/a/53093348)
    
[752458 - chromium - An open-source project to help move the web forward. - Monorail](https://bugs.chromium.org/p/chromium/issues/detail?id=752458)
    
[208667 - getUserMedia does not work in WKWebView-based browsers like Chrome, Firefox.](https://bugs.webkit.org/show_bug.cgi?id=208667)

***

# img 태그 이슈

- ***TL;DR*** 사파리에선 img 태그 안에 선언된 속성보다 큰 이미지를 바인딩하면 제대로 안나옴

```JavaScript
// 동적으로 받아온 이미지의 크기
.some-image {
	width: 30px;
	height: 30px;
}

<img src="some-image" width: "10px", height="10px" />
```

이런 식으로 구현되어 있으면 새로고침해야 나오거나 아예 안나오는 등 제대로 동작하지 않으므로, 이미지를 동적으로 바인딩하는 경우 주의해야 함

***

# 이미지 포맷 변환

- ***TL;DR*** HEIC 이미지를 타사 브라우저나 앱으로 전송(파일 업로드 등)하면 자동으로 JPEG 포맷으로 변환되어 전송됨
- iOS 11 에 HEIC 포맷의 이미지가 추가되었는데, 다른 플랫폼과 서비스에서 해당 이미지를 읽어들이지 못하는 오류가 있었음
- 하여 애플에서 해당 포맷은 전송 시 자동으로 JPEG으로 변환하도록 수정함

#### 참고
[I thought iOS was supposed to convert HEIC images to JPEG automatically behind-t... | Hacker News](https://news.ycombinator.com/item?id=23260987)

