---
title: "toFixed 반올림 오류"
path: js-to-fixed-problem"
date: 2020-08-21 00:00:00
published: true
tags: ['JavaScript']
canonical_url: false
hasImage: false
description: "toFixed 사용 시 간혹 발생하는 반올림 오류 현상을 확인하고, 간단한 해결책을 알아본다."
---

최근 사내 QA 팀에서 확인 요청이 들어온 이슈 하나가 있었다. 웹 서비스 내 각 항목 별 보유자산의 합과 실제 총 보유자산이 소수점 단위에서 조금씩 차이를 보이는 경우가 한 번씩 있다는 것이다.

입사 후 해당 로직을 깊게 들여다본 적이 없었기에 이슈가 들어온 김에 찬찬히 훑어 보았는데, 계산 로직 중 **toFixed** 를 사용하는 부분이 있었다. 예로부터 전설처럼(?) 전해 내려오는 JavaScript의 이상 행동 중 하나로 종종 언급되는 녀석이기에 확인을 해봤더니 역시나 이놈이 실행하는 반올림에 문제가 있었다.

실무 중 이런 경우를 직접 겪어본게 처음이기도 하고, 후에 비슷한 상황을 겪더라도 당황하지 않도록 toFixed의 문제는 무엇이고, 어떻게 해결할 수 있는지 정리해보고자 한다.

***

# 무엇이 문제인가

아래 코드는 실제로 toFixed를 사용함으로써 문제가 발생하는 경우이다. 큰 틀에서 보자면 위의 실무에서 겪은 이슈가 이와 유사한 과정으로 발생하였다.

https://gist.github.com/perade/8fdfc2889061d072e9581b52a33cd881

사용자는 소수점 3자리까지 반올림 후 이를 더한 값인 3.47을 원했으나, 1.2345.toFixed(3)이 1.235가 아닌 1.234를 반환하여 결국 3.469라는 결과를 갖게 되었다. 왜 1.2345를 반올림했는데 1.234가 나온 것일까?

JavaScript의 숫자는 IEEE 754에서 정의한 스펙에 따라 그 형태에 상관없이 항상 **double precision floating point, 즉 64 bit 부동 소수점**으로 저장된다. 때문에 분모가 2의 거듭제곱이 아닌 수는 정확한 값이 아닌 근사치로 표현된다. 실제로 예제에서 사용한 1.2345와 2.2345를 지수 형태로 출력해 보면 아래와 같다.

https://gist.github.com/perade/60028f3c18726c96a97e6935da360e5d

toFixed로 반올림을 시도했을 때 1.2345는 실제로는 1.234에 더 가까운 근사치이고, 2.2345는 실제로는 2.235에 더 가까운 근사치이기 때문에 예제와 같은 결과가 나온다는 것을 알 수 있다.

***

# 해결 방법은 무엇인가

널리 알려진 덧셈 이슈(0.1 + 0.2)의 해결 방법처럼 정수로 반올림 후 다시 원하는만큼 자리수를 조정하는 형태로 해결할 수 있다. 하는 김에 덧셈 이슈도 예제에 한해 해결 가능한 간단한 함수를 만들어 보았다. 아래 코드와 함께 짧은 정리글을 마친다.

https://gist.github.com/perade/70fb8f6fa1dd672534290e60ac6ecfa3

***

### References

- https://stackoverflow.com/questions/588004/is-floating-point-math-broken
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed