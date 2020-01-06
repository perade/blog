---
title: "테스트1"
date: 2020-01-06
published: true
tags: ['Test']
canonical_url: false
description: "한글 테스트 중 / Test Korean"
---

한글 테스트 중

Test Korean..

# Cannot Parse Korean in markdown file

## Description

I'm sorry, but I'm writing here because it seems to be a gridsome or @gridsome/source-filesystem problem.
I executed a [gridsome-starter-blog](https://github.com/gridsome/gridsome-starter-blog) in the local environment to see how gridsome works.
**No code or configure have been modified.**
I wanted to modify the post to Korean because I am Korean.
However, the error occurred when I typed it to Korean, and no post was displayed.
The same error occurs in other CJKs(Chinese, Japanese, Korean).

### Steps to reproduce

Modify the title of `gridsome-starter-blog\content\posts\*.md` file in path as shown below.
It only works if the title contains English or numbers.
_It doesn't work if there's only CJK or if there's CJK and a symbol together_.

```
// AS-IS (work)
---
title: Markdown test file
date: 2019-02-06
published: ...
---

// TO-BE 1 (not work)
---
title: type each word => 테스트 / 测试中 / テスト
date: 2019-02-06
published: ...
---

// TO-BE 2 (not work)
---
title: 테스트..
date: 2019-02-06
published: ...
---

// TO-BE 3 (work)
---
title: 테스트 123
date: 2019-02-06
published: ...
---

// TO-BE 4 (work)
---
title: 테스트 test
date: 2019-02-06
published: ...
---
```

### Expected result

Display post with Korean

### Actual result

Error occurred
```
(node:115948) UnhandledPromiseRejectionWarning: TypeError: Expected "title" to be a string
    at C:\Users\perade\GitHub\gridsome-starter-blog\node_modules\path-to-regexp\index.js:194:13
    at makePath (C:\Users\perade\GitHub\gridsome-starter-blog\node_modules\gridsome\lib\plugins\TemplatesPlugin.js:77:10)
    at C:\Users\perade\GitHub\gridsome-starter-blog\node_modules\gridsome\lib\plugins\TemplatesPlugin.js:205:24
    at SyncBailWaterfallHook.eval [as call] (eval at create (C:\Users\perade\GitHub\gridsome-starter-blog\node_modules\gridsome\node_modules\tapable\lib\HookCodeFactory.js:19:10), <anonymous>:19:16)
    at Collection.updateNode (C:\Users\perade\GitHub\gridsome-starter-blog\node_modules\gridsome\lib\store\Collection.js:127:50)
    at FSWatcher.<anonymous> (C:\Users\perade\GitHub\gridsome-starter-blog\node_modules\@gridsome\source-filesystem\index.js:117:36)
(node:115948) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). (rejection id: 1)
```

### Environment
```
System:
  OS: Windows 10 10.0.18362
  CPU: (8) x64 Intel(R) Core(TM) i7-6700HQ CPU @ 2.60GHz
Binaries:
  Node: 12.14.0 - C:\Program Files\nodejs\node.EXE
  Yarn: 1.21.1 - C:\Program Files (x86)\Yarn\bin\yarn.CMD
  npm: 6.13.4 - C:\Program Files\nodejs\npm.CMD
Browsers:
  Edge: 44.18362.449.0 (I don't know why it looks like this, but my default browser is Chrome 79.0.3945.88.)
npmPackages:
  gridsome: ^0.7.0 => 0.7.12
```
***
Please reply after check. Thank you. 😃 