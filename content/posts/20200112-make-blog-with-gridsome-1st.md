---
title: "Gridsome으로 블로그 개설하기 - 시작하기에 앞서"
slug: "make-blog-with-gridsome-1st"
date: 2020-01-12 00:00:00
published: true
tags: ['Vue', 'Gridsome']
canonical_url: false
description: "Gridsome 이라는 Vue.js 기반 정적 사이트 생성기(Static Site Generator, SSG)와 gh-pages를 활용하여 블로그를 개설하는 방법을 다룬다. 첫 번째 포스팅은 Gridsome이 무엇인지, 다른 정적 사이트 생성기와 비교하여 어떤 특징이 있는지 간략하게 살펴본다."
---

1. [Gridsome으로 블로그 개설하기 - 시작하기에 앞서](https://perade.github.io/blog/make-blog-with-gridsome-1st)
2. [Gridsome으로 블로그 개설하기 - 본격적인 블로그 만들기](https://perade.github.io/blog/make-blog-with-gridsome-2nd)

***

# 개요

이번 글과 다음 글에서는 [Gridsome](https://gridsome.org/) 이라는 Vue.js 기반 정적 사이트 생성기(Static Site Generator, SSG)와 [gh-pages](https://github.com/tschaub/gh-pages)를 활용하여 블로그를 개설하는 방법을 다룬다.

첫 번째 포스팅은 Gridsome과 다른 SSG와의 비교, Gridsome의 장단점 및 선택한 이유 등을 간단하게 설명한다. 두 번째 포스팅은 본격적으로 Gridsome과 gh-pages를 활용하여 어떻게 블로그를 개설하는지 상세하게 설명한다.

***

# Gridsome 이란

![Gridsome](./images/gridsome-logo.png)

위에서 언급했듯 Gridsome은 Vue.js 기반 정적 사이트 생성기이다. React.js 기반의 [Gatsby](https://www.gatsbyjs.org/)를 떠올리면 이해가 쉽다. 실제로 Gridsome에서 밝히기를 Gatsby로부터 상당 부분 영향을 받았다 하고(심지어 영향뿐만 아니라 Gatsby의 대안이라고 공식문서에서 언급한다.), 구현이나 동작 방식 또한 Gatsby에 익숙한 사람들은 내가 지금 Gatsby를 쓰고 있나 할 정도로 유사하다.

때문에 Gridsome 역시 단순한 텍스트 문서뿐만 아니라 GraphQL을 활용한 데이터 조작, PWA 지원, SEO 등 각종 웹 최적화, 웹 표준 준수 등 흔히 말하는 모던 웹 사이트를 구축할 수 있도록 기능을 제공한다.

***

# Gridsome과 다른 정적 사이트 생성기의 비교

앞서 말한 Gatsby 외에도 Node.js 기반의 [HEXO](https://hexo.io/) 등 여러 가지의 SSG 들이 존재한다. 그 중에서도 Vue.js에 친숙한 사람들은 [Vuepress](https://vuepress.vuejs.org/)를 많이 들어봤을 것이다.

Vuepress는 Vue.js 팀에서 공식 제공하는 SSG 로서, Gridsome이나 Gatsby만큼은 아니지만 블로그를 운영하기에는 충분한 기능을 제공한다. 또한 Vue.js 진영의 공식 SSG인 만큼 Vue.js와 관련된 각종 플러그인들과 호환성이 뛰어나다.

반면 Gridsome은 아직 0.X 버전에 머물러 있는 신생(?) SSG 인지라 그만큼 버그도 많고, 레퍼런스와 커뮤니티도 타 SSG에 비해 현저히 부족하다. 특히 i18n 지원이 아직 부족하여 공식 레포지토리에서 영어권 외 국가의 개발자들이 올린 이슈도 심심찮게 볼 수 있다. SSG가 블로그 개설에 많이 활용되는 것을 생각하면 꽤나 크리티컬한 이슈다.

그럼에도 불구하고 블로그 개설을 위해 Gridsome을 선택한 이유는 **1. Vue.js + GraphQL 환경을 손쉽게 경험해보고 싶었고, 2. 모던 웹을 위한 각종 기능들 및 이미지 처리, 동적 라우팅 등을 꽤 괜찮은 수준으로 자체 지원**해주기 때문이다.

정답은 아니지만 블로그를 개설하고자 할 때 아래와 같이 생각하면 참고가 될 것이다.

```
// 1. 레퍼런스 & 커뮤니티 활성도

Gatsby >>>>>>> Vuepress > Gridsome

// 2. 기능의 다양성

Gatsby = Gridsome >>> Vuepress

// 3. 기술의 성숙도(안정성)

Gatsby >>> Vuepress >>> Gridsome

// 4. 그래서 결론은?

React.js에 친숙하거나 관심이 있다
-> Gatsby

Vue.js에 친숙하고, 블로그 정도는 별다른 기능 없이 컴팩트하게 가도 괜찮다.
여차하면 내가 구현하지 뭐
-> Vuepress

Vue.js에 친숙하고, GraphQL에 관심이 있으며, 아무리 블로그라도 웹 트렌드를
따라가야한다. 근데 내가 죄다 구현하긴 귀찮다
-> Gridsome
```

***

이번 포스팅에서는 Gridsome이 무엇인지, 또 다른 정적 사이트 생성기와 비교하여 어떤 특징이 있는지 간략하게 살펴보았다. 다음 포스팅에서는 본격적으로 Gridsome을 활용하여 어떻게 블로그를 개설하는지 상세하게 알아보도록 하겠다.

***

### References

- https://gridsome.org/
- https://vuepress.vuejs.org/
