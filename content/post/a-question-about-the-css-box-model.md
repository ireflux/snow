---
title: "子元素margin-top影响父元素问题"
date: 2018-5-10T10:33:00+8:00
lastmod: 2018-5-10Y10:33:00+8:00
draft: false
categories: ["前端"]
tags: ["我的前端之路"]
author: "sherry"
---
今天遇到了一个问题，是关于前端CSS子元素的margin-top会影响父元素的问题。搞了好长时间也不知道怎么回事，最后终于解决了，写出来权当做个记录。

我的HTML嵌套关系如下：

```html
<header id="site-head">
    <a id="blog-logo">
        <div class="logo" style="background: url(/resources/images/qyc.jpg)"></div>
    </a>
</header>
```

<!--more-->

父元素CSS：

```css
#site-head{
    max-width: 100%;
    background-image: url("/resources/images/header.jpg");
    background-size: cover;
}
```

子元素CSS：

```css
#blog-logo {
    display: block;
    max-width: 100px;
    margin: 50px auto 0;
    text-align: center;
}
```

当子元素中margin-top为50px时，神奇的一幕发生了，子元素并没有相对于父元素向下移动50px，而是连带着父元素一起向下移动了50px。

在Chrome的Developer tools中查看时，看到就连body都向下移动了50px！之后尝试在父元素中设置margin或body中设置都没有用，在父元素中设置padding也没有效果。

之后搜索到的css2.1盒模型中规定的内容如下：

>In this specification, the expression collapsing margins means that adjoining margins (no non-empty content, padding or border areas or clearance separate them) of two or more boxes (which may be next to one another or nested) combine to form a single margin.

所有毗邻的两个或更多盒元素的margin将会合并为一个margin共享之。毗邻的定义为：同级或者嵌套的盒元素，并且它们之间没有非空内容、Padding或Border分隔。因为嵌套也属于毗邻，所以在样式表中优先级更高的。因此子元素中的margin覆盖了父元素的margin。

之后找到的解决方法如下：

1. 父级或子元素使用浮动或者绝对定位absolute浮动或绝对定位不参与margin的折叠
2. 父级overflow:hidden;
3. 父级设置padding（破坏非空白的折叠条件）**//对于这条表示怀疑，我之前设置的padding并没有起到作用(**
4. 父级设置border

最终我用的`父级overflow:hidden;`完美解决问题！