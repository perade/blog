---
title: "Gridsome으로 블로그 개설하기 - 본격적인 블로그 만들기"
path: "make-blog-with-gridsome-2nd"
date: 2020-01-12 01:00:00
published: true
tags: ['Vue.js', 'Gridsome']
canonical_url: false
hasImage: true
description: "Gridsome 이라는 Vue.js 기반 정적 사이트 생성기(Static Site Generator, SSG)와 gh-pages를 활용하여 블로그를 개설하는 방법을 다룬다. 두 번째 포스팅은 예제를 통해 실제로 블로그를 구축해 보면서 어떻게 블로그를 개설할 수 있는지 자세히 살펴본다."
---

1. [Gridsome으로 블로그 개설하기 - 시작하기에 앞서](https://perade.github.io/blog/make-blog-with-gridsome-1st)
2. [Gridsome으로 블로그 개설하기 - 본격적인 블로그 만들기](https://perade.github.io/blog/make-blog-with-gridsome-2nd)

***

# 개요

이전 포스팅에서 [Gridsome](https://gridsome.org/)에 대해 간략하게 살펴보았다. 이번 글에서는 실제로 Gridsome과 [gh-pages](https://github.com/tschaub/gh-pages)를 활용하여 어떻게 블로그를 개설하는지 예제를 통해 설명하고자 한다.

***

# Gridsome 설치

Gridsome을 사용하려면 8.3버전 이상의 Node.js가 설치되어 있어야 한다. 만약 Node.js가 설치되어 있지 않다면 [여기](https://nodejs.org/ko/)를 참고하여 Node.js부터 설치하도록 한다.

Node.js가 설치되었다면 터미널에서 아래 명령어를 통해 Gridsome을 설치한다.

```
// NPM을 사용할 경우
npm install --global @gridsome/cli

// YARN을 사용할 경우
yarn global add  @gridsome/cli
```

현재 시점(20.01.12) 기준 0.7.12 버전이 설치된다. 이제 Gridsome을 사용할 준비가 끝났다.

***

# 블로그 프로젝트 만들기

직접 `gridsome create my-blog` 와 같은 명령어를 통해 0에서부터 시작하는 방법도 있지만, 아래와 같이 Gridsome에서 제공하는 스타터를 통해 쉽게 시작할 수도 있다. 본 글에서는 Gridsome에서 공식 제공하는 [Gridsome Blog Starter](https://github.com/gridsome/gridsome-starter-blog)를 활용해 블로그를 개설해보도록 하겠다. 최소한의 플러그인만 붙어 있어 나중에 커스터마이징 하기가 용이하기 때문이다.

![image](./images/gridsome-starters.png)

Gridsome Blog Starter 레포지토리에 들어가보면 스타터를 어떻게 설치하는지 아래와 같이 설명되어 있다.

```
gridsome create my-blog https://github.com/gridsome/gridsome-starter-blog.git

cd my-blog

gridsome develop
```

차례대로 실행 후 브라우저에서 localhost:8080 으로 접속했을 때 아래와 같은 화면이 나온다면 제대로 설치 및 실행이 된 것이다.

![image](./images/gridsome-blog-starter-index.png)

이제 자신의 GitHub에 my-blog 레포지토리를 올려 연동시키면 블로그 개설을 위한 사전 준비는 끝난 셈이다.

***

# 프로젝트 구조 확인 후 블로그 포스팅하기

프로젝트 세팅이 끝났으니 이제 본격적으로 글을 작성하여 블로그에 포스팅을 해 볼 것이다. 프로젝트의 기본 구조는 아래와 같다.

- **content/posts**: *.md 파일에 포스팅 할 글을 작성
- **src/pages**: 이 폴더의 파일 기준으로 페이지가 작성되고, 라우팅이 된다. 예를 들어 About.vue가 있으면 my-blog-domain/about으로 페이지에 접근하는 식이다.
- **src/layouts**: 페이지의 기본 레이아웃
- **src/components**: 페이지를 꾸미는 컴포넌트들은 여기서 구현하면 된다.
- **src/templates**: [컬렉션](https://gridsome.org/docs/collections/) 노드들의 단일 페이지를 구성하는데 사용된다. 여기서는 블로그 포스트를 구성한다고 생각하면 쉽다.

content/posts에 sample-post.md 파일을 작성하면 해당 파일을 @gridsome/source-filesystem 플러그인을 통해 my-blog-domain/sample-post 페이지로 만들어주는 식이다. 

sample-post.md 파일을 생성 후 아래와 같이 작성한다.

```
---
title: Sample post
date: 2020-01-12
published: true
tags: ['Markdown', 'Test files']
canonical_url: false
description: "샘플 포스트입니다."
---

샘플 포스트입니다.

마크다운 양식에 맞춰 작성하시기 바랍니다.
```

**gridsome.config.js**를 살펴보면 마크다운 내의 front-matter들이 페이지 작성에 어떤 식으로 사용되고 있는지 확인해볼 수 있다.

```
templates: {
  Post: '/:title',
  Tag: '/tag/:id'
},

plugins: [
  {
    // Create posts from markdown files
    use: '@gridsome/source-filesystem',
    options: {
      typeName: 'Post',
      path: 'content/posts/*.md',
      refs: {
        // Creates a GraphQL collection from 'tags' in front-matter and adds a reference.
        tags: {
          typeName: 'Tag',
          create: true
        }
      }
    }
  }
],
```

title은 해당 포스트의 URL로 사용하고, tags는 태그 별로 모아볼 수 있는 기능을 제공한다. date, description 등 다른 front-matter들도 GraphQL을 통해 페이지 메타 정보 등에 활용할 수 있다.

sample-post.md 파일 작성을 완료한 후 다시 브라우저로 돌아가 아래와 같이 제대로 반영이 되었는지 확인한다.

![image](./images/sample-post-card.png)

![image](./images/sample-post-detail.png)

위에서 언급한대로 태그를 클릭하면 해당 태그로 묶여있는 포스트들도 확인할 수 있다.

![image](./images/tag-page.png)

***

# GitHub Pages를 활용하여 실제 블로그 개설하기

이제 외부에서도 접근할 수 있도록 실제로 블로그를 개설할 차례이다. 여러 호스팅 서비스들이 있지만 개발자에게 제일 만만한(?) [GitHub Pages](https://pages.github.com/)를 활용해 블로그를 개설해보도록 하겠다.

GitHub Pages를 통해 호스팅하는 방법은 크게 **my-name.github.io** 레포지토리를 만들어서 띄우는 방법과 **gh-pages** 를 활용해 띄우는 방법이 있는데, 본 글에서는 후자의 방법을 사용할 것이다.

우선 아래 명령어를 참고하여 gh-pages를 설치한다.

```
npm install gh-pages / yarn add gh-pages
```

그 다음 **gridsome.config.js** 파일에 아래 내용을 추가한다.

```
siteUrl: 'https://my-name.github.io',
pathPrefix: '/my-blog'
```

마지막으로 **package.json**의 scripts에 아래와 같이 명령어를 추가한다.

```
"deploy": "gridsome build && gh-pages -d dist"
```

설정 완료 후 터미널에서 `npm run deploy`를 실행하면 `gridsome build`의 결과물인 dist 폴더가 생긴다. 그리고 이를 자동으로 gh-pages 브랜치에 푸시하고, 이 브랜치를 GitHub Pages의 소스로 설정해준다.

아래는 현재 이 블로그의 설정이다. gridsome.config.js에서 설정한대로 https://my-name.github.io/my-blog 가 제대로 퍼블리시 되어있는지 확인한다.

![image](./images/blog-github-pages-setting.png)

정상적으로 배포가 완료되었다면 아래처럼 접근할 수 있을 것이다. (빠르면 바로 GitHub Pages에 배포가 반영되지만 길면 몇 분정도 걸린다.)

![image](./images/gridsome-blog-after-deploy.png)

***

# 몇 가지 문제점

블로그 개설을 손쉽게 완료했지만, 말 그대로 스타터일 뿐 디자인이나 사용자 편의를 위한 개별 구현이 필요하다. 아래는 필자가 블로그를 개설하고 만져 보면서 맞닥뜨린 문제점들이다. [이전 글](https://perade.github.io/blog/make-blog-with-gridsome-1st)에서 언급했듯이 i18n과 관련한 버그가 좀 있다.

* 마크다운의 front-matter 중 title에 한글만 또는 한글+기호만 사용하면 파싱 오류가 발생함
  - 해결: [이슈](https://github.com/gridsome/gridsome/issues/918)를 올렸는데 기다려보고 딱히 반응이 없으면 직접 해결하거나 우회해야할 듯 싶다. @gridsome/source-filesystem에서 glob 패턴을 사용하여 path를 생성하는데 여기에 버그가 있는것으로 추정된다. 쉽게 우회하는 방법은 title에 영문이나 숫자를 섞어서 쓰는 것이다.
* 위와 연계된 이슈인데, front-matter에 영문이나 숫자를 섞어 한글 title을 설정하더라도 path 생성 시 한글을 제대로 인식하지 못하기 때문에 URL이 의도치 않게 표현된다.
  - ```title: 2019년 회고, URL: my-blog-domain/2019/``` 
  - 해결: front-matter에 slug를 추가하여 여기에 URL에 쓰일 제목(영문+숫자)을 넣고, gridsome.config.js의 templates > Post를 '/:slug'로 변경하면 된다.
  ```
  // front-matter in *.md file
  title: 2019년 회고
  slug: 2019-retrospection

  // gridsome.config.js
  templates: {
    Post: '/:slug',
    Tag: '/tag/:id'
  }
  ```
  하지만 slug가 Gridsome 0.8 버전부터는 Deprecation 될 예정이기 때문에 첫 번째의 근본적인 이슈가 해결되어야 한다.
* GraphQL의 Post API에서 cover_image 파라미터가 있으면 값을 무조건 넘겨야지만(즉, null을 허용하지 않는) 동작하는 [오류](https://github.com/gridsome/gridsome/issues/471)가 있다. 0.7버전부터는 해결되었다고 하는데, 필자를 포함하여 아직도 동일한 이슈가 재현되는 사람들이 있는것으로 보아 완벽히 해결된 건 아닌듯 하다.
  - 해결: 만약 위 이슈가 발생했다면 GraphQL 스키마를 변경하거나 cover_image 파라미터를 삭제해야 한다.

***

# 마치며..

이번 포스팅에서는 Gridsome을 활용하여 어떻게 블로그를 개설하는지 예제를 통해 상세하게 알아보았다. Gridsome은 다양한 기능을 지원하지만 비교적 신생(?) 프레임워크인 만큼 다른 SSG에 비해 아직 안정성이나 커뮤니티 활성도가 떨어지는 편이다. 하지만 좀 더 시간이 지나고 개발이 꾸준히 진행된다면 Vue.js 진영에서 크게 반길 만한 SSG가 될 수 있을 것이라 기대한다.

Gridsome 관련 궁금한 내용은 [공식 문서](https://gridsome.org/docs/)나 [Discord](https://discord.gg/daeay6n)를 활용하면 도움을 받을 수 있을 것이다. 두 편에 걸친 긴 포스팅을 읽어 주셔서 감사하다는 마음을 전하며 **Gridsome으로 블로그 개설하기** 포스팅을 마친다. 😃

***

### References

- https://gridsome.org/
- https://github.com/gridsome/gridsome-starter-blog
- https://github.com/tschaub/gh-pages
