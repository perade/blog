---
title: "[Vue.js] computed vs watch - computed와 watch의 차이점"
path: "vue-computed-vs-watch"
date: 2020-02-17 00:00:00
published: true
tags: ['Vue']
canonical_url: false
description: "Vue.js에서 데이터 변경에 반응하여 후속 동작을 취해야 할 때 computed와 watch 속성을 흔히 사용한다. 언뜻 비슷해 보이는 두 속성의 차이점은 무엇인지, 상황 별로 어느 것을 사용해야 적합한지 알아본다."
---

Vue.js를 처음 학습할 때 가장 헷갈렸던 것중 하나가 computed와 watch를 선택해서 사용해야 하는 상황이었다. 언뜻 보면 비슷한 동작을 하는 것 같은데 차이가 있을테니 두 개가 있을 것이고, 이름도 뭔가 계산된거 같은 이름과 지켜보는 듯한 이름으로 차이가 있다. 공식 문서에 두 속성에 대한 개념과 차이점이 잘 정리되어 있지만 되새기는 차원에서 글을 작성해 본다.

***

# computed

computed는 데이터의 변경에 반응하여 특정 값을 반환해주는 일종의 **getter 함수**이다. 아래는 공식 문서의 예제 코드이다.

https://gist.github.com/perade/cc8d98bddb8e4f96ba3878ce9d347b5f

예제를 실행해보면 `message`의 값에 따라 `reversedMessage`가 반환하는 값이 변경됨을 알 수 있다. `reversedMessage`라는 **getter 함수**가 `message`에 의존성을 가지고 있기 때문인데, 이것이 바로 computed 속성의 특징이다.

또한 computed의 장점중 하나는 의존성을 가진 데이터가 변경되지 않으면 getter 호출 시 내부 로직 연산을 다시 하지 않고, 이미 계산되어 있는 결과를 즉시 반환한다는 점이다. 이는 [Vue가 데이터를 추적하기 위해 사용하는 방식](https://kr.vuejs.org/v2/guide/reactivity.html#%EB%B3%80%EA%B2%BD-%EB%82%B4%EC%9A%A9%EC%9D%84-%EC%B6%94%EC%A0%81%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95) 덕분인데, 연산이 부담스러운 데이터 같은 경우 이러한 특성을 잘 활용하면 불필요한 연산을 줄일 수 있다. 매번 연산이 필요할 경우엔 method를 활용하면 된다.

***

# watch

watch는 computed와 비슷하지만 **특정 데이터가 변경되었을 때 지정한 Callback 함수를 실행**하는, 즉 사용 목적이 다른 속성이다. 아래는 공식 문서의 예제 코드를 일관성을 위해 `message`를 활용한 예제로 변경한 것이다.

https://gist.github.com/perade/85b643e0153ae5bba53ddd9d27cc6e32

예제를 보면 결과는 computed를 사용했을 때와 동일하지만 코드가 길어지고(습관적으로 watch를 풀어서 사용한 탓도 있다.) 복잡도가 더 올라갔다. 하지만 computed 사용이 더 적합한 예제를 다뤄서 그렇지, 특정 데이터가 변경되었을 때 API 호출 등 특정 동작을 실행해야하는 상황은 watch가 더 적합하다고 Vue에서는 언급하고 있다. 물론 대부분의 경우 computed가 더 적합하다는 말도 함께 언급하고 있지만 말이다.

***

# 정리

간단하게 computed와 watch 속성의 차이점에 대해 알아보았다. 둘 다 데이터 변경에 반응하는 특징을 갖고 있지만, 사용 목적이 다름을 인지하고 상황에 맞게 사용하려는 노력이 필요하다. 정리하자면 데이터 변경 시 특정 동작을 취해야 하는 상황은 watch, 그 외 상황은 대체로 computed가 적합하다고 할 수 있다.

***

### References

- https://kr.vuejs.org/v2/guide/computed.html
- https://medium.com/@jeongwooahn/vue-js-watch%EC%99%80-computed-%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%99%80-%EC%82%AC%EC%9A%A9%EB%B2%95-e2edce37ec34