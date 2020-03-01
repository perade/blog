---
title: "MutationObserver - DOM 변경 감지"
path: "mutation-observer"
date: 2020-02-04 00:00:00
published: true
tags: ['JavaScript', 'HTML']
canonical_url: false
description: "Web API 중 하나인 MutationObserver의 기능과 용도를 간단한 예제 코드와 함께 알아본다."
---

예전에 이미지 업로드를 구현할 일이 있었는데, 이미지가 10장을 초과하면 최대 10장까지라는 토스트 메시지를 표시하는 것과 이미지를 1024 이하로 resizing 해야 하는 요구사항이 있었다.

토스트를 호출하는 로직과 img와 canvas를 활용한 이미지 resizing 로직을 구현 후 테스트를 진행했는데, 불필요한 렌더링이 발생하여 페이지가 깜빡거리거나 토스트가 버벅이며 올라오는 등의 현상이 있었다.

이는 별개의 두 로직이 DOM에 동시에 접근하다보니 생긴 현상이었다. 문제 해결을 위해 조사하던 중 MutationObserver 라는 Web API가 있다는 것을 알게 됐다.

***

# MutationObserver

[MutationObserver](https://developer.mozilla.org/ko/docs/Web/API/MutationObserver)는 DOM 트리의 변경을 감시하는 기능을 제공한다. 아래는 MDN에서 제공하는 예제 코드이다.

https://gist.github.com/perade/def10d403bb2cbc16313357934a01ca3

코드를 보면 사용이 굉장히 간단한 것을 확인할 수 있다. 큰 흐름은 아래와 같다.

- 감시할 대상을 정함
- 감시자를 생성
- 필요에 따라 감시를 ON / OFF

특정 DOM Node를 observe 함수를 실행하여 감시하고 있다가 해당 Node가 변경되면 Callback 함수를 실행하여 원하는 동작을 취할 수 있다. observe 함수 실행 시 옵션을 같이 전달할 수 있는데, 옵션 이름을 보면 해당 옵션이 어떤 목적인지 쉽게 유추할 수 있다. (attributes: true 는 id, class 등 해당 Node의 속성 변경을 감시, childList: true 는 해당 Node의 자식들이 변경되는지 감시 등)

***

다시 예전으로 돌아가서, 필자는 당시 MutationObserver를 활용하여 이미지 resizing을 위한 canvas 엘리먼트가 생성되었음을 확인한 후 필요에 따라 토스트를 호출하는 식으로 로직을 수정하여 문제를 해결하였다.

이처럼 DOM 변경에 따라 실행되어야 하는 동작이 있을 경우 MutationObserver를 유용하게 활용할 수 있다. 또한 Vue.js나 React.js와 같은 SPA 도구들에 의존하는 경우 가상 DOM을 거치기 때문에 애플리케이션의 규모나 복잡도가 높으면 각 컴포넌트들의 실제 DOM 반영 시점을 알기 어려운 경우가 종종 있다. 이러한 경우 MutationObserver를 활용한 디버깅용 Wrapper를 만들어 필요에 따라 활용할 수도 있다.

***

### References

- https://developer.mozilla.org/ko/docs/Web/API/MutationObserver