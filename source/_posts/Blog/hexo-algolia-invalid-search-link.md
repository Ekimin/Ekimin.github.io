---
title: Hexo 博客 Algolia 插件搜索到的链接点击无效（直接跳转到主页）
tags:
  - Hexo
  - Algolia
  - 博客
categories: 博客
date: 2017-06-12 17:20:46
updated:
comments:
permalink:
---


解决了Hexo 博客 Algolia 插件搜索到的链接点击无效的问题。

<!--more-->

出现这个问题的原因是hexo-algolia的作者并没有把post.path进行index，所以data.path是undefined。

解决方法：
在node_modules/hexo-algolia/lib， 找到command.js 如下：

```js
var storedPost = _.pick(data, ['title', 'date', 'slug', 'content', 'excerpt', 'permalink']);
```

添加 'path' 到数组中，变成：

```js
var storedPost = _.pick(data, ['title', 'date', 'slug', 'path', 'content', 'excerpt', 'permalink']);
```

然后重新生成 algolia 索引，发布博客。