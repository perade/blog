---
title: "[번역] await vs return vs return await"
path: "await-vs-return-vs-return-await"
date: 2020-03-04 00:00:00
published: true
tags: ['JavaScript', 'Translation']
canonical_url: false
hasImage: false
description: "async 함수를 작성할 때 사용하는 await, return, return await 각각의 차이점을 알아본다."
---

> Jake Archibald의 [await vs return vs return await](https://jakearchibald.com/2017/await-vs-return-vs-return-await/) 번역글입니다. 굳이 이 글이 아니더라도 모든 번역글은 역자의 의도와 상관없이 원문의 내용과 다르게 전달될 수 있으니 원문도 같이 보시는 걸 권해드립니다.

async 함수를 작성할 때 사용하는 `await`, `return`, `return await`의 차이점을 알고, 올바른 선택을 하는 것이 중요하다. 아래 예제를 보자.

```
async function waitAndMaybeReject() {
  // Wait one second
  await new Promise(r => setTimeout(r, 1000));

  // Toss a coin
  const isHeads = Boolean(Math.round(Math.random()));

  if (isHeads) return 'yay';
  throw Error('Boo!');
}
```

이 함수는 반반의 확률로 Fulfilled(이행) 상태가 되어 'yay'를 던져주거나 Rejected(실패) 상태가 되어 에러를 던지는 promise를 반환한다. 이걸 몇 가지 다른 방식으로 사용해보자.

***

## Just calling

```
async function foo() {
  try {
    waitAndMaybeReject();
  }
  catch (e) {
    return 'caught';
  }
}
```

위와 같이 `foo`를 호출하면 항상 **undefined인 Fulfilled 상태의 promise를 곧바로 리턴**할 것이다. `waitAndMaybeReject()`를 await 하거나 리턴하지 않았기 때문이다.

## Awaiting

```
async function foo() {
  try {
    await waitAndMaybeReject();
  }
  catch (e) {
    return 'caught';
  }
}
```

`foo`를 호출하면 이번엔 **항상 1초를 기다린 뒤 undefined나 'caught'인 Fulfilled 상태의 promise**를 리턴할 것이다. 위 코드에서 `waitAndMaybeReject()`를 await 하기 때문에 해당 함수가 Rejected 상태의 promise를 리턴하면 catch 블록이 실행되어 'caught'를 리턴하지만, Fulfilled 상태의 promise를 리턴하면 여전히 await만 할 뿐 아무 동작을 하지 않기 때문에 yay를 얻을 수 없다.

## Returning

```
async function foo() {
  try {
    return waitAndMaybeReject();
  }
  catch (e) {
    return 'caught';
  }
}
```

`foo`를 호출하면 **항상 1초를 기다린 뒤, 'yay'를 가진 Fulfilled 상태나 Error('Boo!')를 throw하는 Rejected 상태의 promise**를 리턴할 것이다. `waitAndMaybeReject()`를 await 하지 않고 바로 리턴하기 때문에 catch 블록은 절대 실행되지 않는다.

## Return-awaiting

```
async function foo() {
  try {
    return await waitAndMaybeReject();
  }
  catch (e) {
    return 'caught';
  }
}
```

`foo`를 호출하면 **항상 1초를 기다린 뒤, 'yay'나 'caught'를 가진 Fulfilled 상태의 promise**를 리턴할 것이다. `waitAndMaybeReject()`를 await 했기 때문에 해당 함수가 Rejected 상태의 promise를 리턴하면 catch 블록이 실행되어 'caught'를 리턴하고, Fulfilled 상태이면 'yay'를 리턴한다.

***

위의 내용들이 혼란스러우면 아래와 같이 두 단계로 생각하면 좀 더 쉬울 것이다.

```
async function foo() {
  try {
    // waitAndMaybeReject()의 결과가 정해질 때까지 기다린 후,
    // 그 값을 fulfilledValue에 할당
    const fulfilledValue = await waitAndMaybeReject();

    // 그리고 fulfilledValue를 리턴
    return fulfilledValue;
  }
  catch (e) {
    // 만약 waitAndMaybeReject()가 Rejected 상태면
    // catch 블록이 실행되기 때문에 'caught'를 리턴
    return 'caught';
  }
}
```

[Note](https://github.com/eslint/eslint/blob/master/docs/rules/no-return-await.md): async 함수의 리턴값은 항상 Promise.resolve로 wrapped 되어 있기 때문에 return await는 실효성이 없다. 다만 try/catch 구문에서는 다른 Promise 함수의 오류를 catch 하기 위해 사용될 수 있다.