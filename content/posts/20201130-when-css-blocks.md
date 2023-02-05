---
title: "[번역] When CSS Blocks"
path: "when-css-blocks"
date: 2020-11-30 00:00:00
published: true
tags: ['Translation', 'HTML', 'CSS']
canonical_url: false
hasImage: true
description: "Vue.js의 메이저 버전을 준비하며 얻은 교훈들"
---

> Tim Kadlec의 [When CSS Blocks](https://timkadlec.com/remembers/2020-02-13-when-css-blocks/) 번역글입니다. 굳이 이 글이 아니더라도 모든 번역글은 역자의 의도와 상관없이 원문의 내용과 다르게 전달될 수 있으니 원문도 같이 보시는 걸 권해드립니다.

이 글에서는 ***preload***를 왜 주의해서 사용해야 하는지, 또 Document 순서가 성능에 어떻게 중요한 영향을 미칠 수 있는지 실제 사례에 기반하여 설명하고자 한다(참고: [Harry Roberts가 상세하게 설명한 글](https://csswizardry.com/2018/11/css-and-network-performance/)이 있다).

필자는 [Filament Group](https://www.filamentgroup.com/)의 열렬한 팬이다. 그들은 엄청난 양의 고품질 작업물들을 생산하여 지속적으로 웹 개선을 위한 리소스들을 제공하고 있다. [loadCSS](https://github.com/filamentgroup/loadCSS)는 그 작업물 중 하나인데, 이 프로젝트는 필자가 오랫동안 중요하지 않은 CSS를 로드하는 방법으로 권장해온 것이다.

[Filament Group](https://www.filamentgroup.com/)의 작업 방향이 [바뀌긴 했지만](https://www.filamentgroup.com/lab/load-css-simpler/), 필자는 여전히 운영 환경에서 사용하고 있다.

필자가 주목한 패턴 중 하나는 ***preload/polyfill*** 패턴이다. 이 패턴에서는 스타일시트를 preload로 사전에 로드한 다음, onload 이벤트를 사용하여 브라우저가 준비되었을 때 스타일시트로 다시 돌아가 작업을 진행할 수 있다. 아래는 preload의 예시이다.

```html
<link rel="preload" href="path/to/mystylesheet.css" as="style" onload="this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="path/to/mystylesheet.css"></noscript>
```

모든 브라우저가 preload 기능을 지원하는 것은 아니기 때문에, loadCSS는 다음과 같이 polyfill을 제공한다.

```html
<link rel="preload" href="path/to/mystylesheet.css" as="style" onload="this.rel='stylesheet'">
<noscript>
  <link rel="stylesheet" href="path/to/mystylesheet.css">
</noscript>
<script>
/*! loadCSS rel=preload polyfill. [c]2017 Filament Group, Inc. MIT License */
(function(){ ... }());
</script>
```

***

## 네트워크 우선 순위에서 벗어남


