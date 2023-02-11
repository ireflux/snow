---
title: "子元素margin-top影响父元素问题"
date: 2018-05-10
lastmod: 2018-05-10
draft: false
categories: ["Clavicula Salomonis"]
tags: ["CSS"]
author: "sherry"
---
今天遇到了一个问题，是关于前端 CSS 子元素的 margin-top 会影响父元素的问题。搞了好长时间也不知道怎么回事，最后终于解决了，写出来权当做个记录。

我的 HTML 嵌套关系如下：

```html
<header id="site-head">
    <a id="blog-logo">
        <div class="logo" style="background: url(/resources/images/qyc.jpg)"></div>
    </a>
</header>
```

<!--more-->

父元素 CSS：

```css
#site-head{
    max-width: 100%;
    background-image: url("/resources/images/header.jpg");
    background-size: cover;
}
```

子元素 CSS：

```css
#blog-logo {
    display: block;
    max-width: 100px;
    margin: 50px auto 0;
    text-align: center;
}
```

当子元素中 margin-top 为 50px 时，神奇的一幕发生了，子元素并没有相对于父元素向下移动 50px，而是连带着父元素一起向下移动了 50px。

在 Chrome 的 Developer tools 中查看时，看到就连 body 都向下移动了 50px！之后尝试在父元素中设置 margin 或 body 中设置都没有用，在父元素中设置 padding 也没有效果。

之后搜索到的 css2.1 盒模型中规定的内容如下：

>In this specification, the expression collapsing margins means that adjoining margins (no non-empty content, padding or border areas or clearance separate them) of two or more boxes (which may be next to one another or nested) combine to form a single margin.

所有毗邻的两个或更多盒元素的 margin 将会合并为一个 margin 共享之。毗邻的定义为：同级或者嵌套的盒元素，并且它们之间没有非空内容、Padding 或 Border 分隔。因为嵌套也属于毗邻，所以在样式表中优先级更高的。因此子元素中的 margin 覆盖了父元素的 margin。

之后找到的解决方法如下：

1. 父级或子元素使用浮动或者绝对定位 absolute 浮动或绝对定位不参与 margin 的折叠
2. 父级 overflow:hidden;
3. 父级设置 padding（破坏非空白的折叠条件）**//对于这条表示怀疑，我之前设置的 padding 并没有起到作用(**
4. 父级设置 border

最终我用的`父级 overflow:hidden;`完美解决问题！