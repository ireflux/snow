---
title: "博客迁移记录"
date: 2019-04-23
lastmod: 2019-04-23
draft: false
categories: ["随笔"]
author: "sherry"
---
前些天看到霍矩大佬的博客主题很漂亮，名字叫做Even。从他的博客里第一次了解到Hugo，加上对Golang一直挺有好感，就一直想搞一个来玩。之后偶尔逛论坛看到有人说文章多了Hexo生成静态页面比较慢，然后下面好多V(v2ex)友都推荐Hugo，我自己的博客用的也是Hexo，大概30篇的博文要生成9秒，确实有点慢，加上一直不喜欢Nodejs的臃肿，博客换成Hugo的想法就更强烈了。今天终于有时间来迁移博客，以下权当做个记录。

<!--more-->

博客的迁移是个体力活，主要还是文章的迁移，由于Hugo的标头(e.g. title,tags,category)的写法和Hexo略有不同，因此需要将文章中的标头全改成Hugo能识别的写法，另外还有一些文章里用到了Hexo里带的一些写法，也要改掉。看来以后还是要尽量使用Markdown原生语法，不然迁移博客会很麻烦:(

Github pages上分为 `User/Organization pages` 和 `Project pages`，配置的方式Hugo的[文档](https://gohugo.io/hosting-and-deployment/hosting-on-github/)写的很详细，选择一种按照步骤走就可以了。

我的master分支放的是源文件，gh-pages放的是生成的静态文件，正如Hugo官网所说：这样做的好处是使其有独立的版本控制。我原来用Hexo的User pages保留了，现在上面只挂一个resume，其他文章全都迁移到了Project pages上，也就是用Hugo的这个博客。体验确实很好，静态文件基本上是秒生成，完美！

## 参考资料

> [host on github](https://gohugo.io/hosting-and-deployment/hosting-on-github/)  
> [official documentation for custom domains](https://help.github.com/articles/using-a-custom-domain-with-github-pages/)