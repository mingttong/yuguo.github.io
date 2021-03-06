---
layout: post
title: 自适应按钮
date: 2010-10-28 09:03
comments: true
categories: [front-end]
---

按钮大小或者宽度根据内容而自适应主要技术难点在于_圆角边框_和_渐变背景_在所有浏览器的兼容。本文分两部分来说明各种解决方案。

本文不是所有的解决方案大全，只是选取介绍其中较优的方式。一些空标签和嵌套过多，或者HTTP请求过多的方法也能完成任务，但这些糟糕的方法即使介绍出来也是没有人使用的，就不浪费时间了。
[所有例子的DEMO在这里。](http://yuguo.us/wp-content/blogs.dir/5/files/2010/10/demo/)
关于圆角的、渐变的按钮，常用的技术方案有：

*   使用图片

    *   非自适应的图片
    *   自适应的图片
*   用嵌套的div+border模拟
*   用CSS3渐进增强

## 非自适应的图片

bt-x中的x为按钮中文字的个数,代码如下：

    &lt;button class="bt-2"&gt;确认&lt;/button&gt;

CSS代码：

    .bt-2,

    .bt-3,

    .bt-4{text-align:center;border:0 none;background-color:transparent;padding:0 .2em;margin-right:8px;height:21px;line-height:21px;background-image:url(btn.png);background-repeat:no-repeat;cursor:pointer;}

    .bt-2{width:46px;background-position:0 -24px;}

    .bt-3{width:57px;background-position:0 -48px;}

    .bt-4{width:69px;background-position:0 -72px;}

Tips：

1.  使用png-8的png图片，而不是png24/32（不同的浏览器可能会有差异，实际上他们是一样的），这样就可以直接支持IE6了。
2.  按钮背景的问题也顺便解决了。注意这个方法中的按钮背景是不用平铺的，所以可以放在雪碧图中的任意地方。可以说是“sprite-friendly”。

## 自适应的图片

如果希望按钮能够随着文字增多而自动拉长，那么思路就是按钮只能有一个class比如button，而不是像上一例那样有多个class。这时候最优的解决方案是利用CSS3做圆角。Wordpress的后台button就是一个很好的例子。

    .button {background:#f2f2f2 url(http://xx.png) repeat-x;text-shadow:#fff 0 1px 0;

    cursor:pointer;border-width:1px;border-style:solid;-moz-border-radius:11px;-khtml-border-radius:11px;-webkit-border-radius:11px;border-radius:11px;

    }

效果在Chrome和Firefox下是相当好看的，在IE下的方方正正+渐变也OK。

## 用嵌套的div+border模拟

这是根据Google的按钮思路来的，详情见[YT](http://www.99css.com/)的[自适应圆角](http://www.99css.com/?p=146)，很专业的分析文章。

## 利用CSS3做的背景色渐变

    background: #ccc; background: -moz-linear-gradient(top, #fff, #d2d2d2);background: -webkit-gradient(linear, 0 0, 0 100%, from(#fff), to(#d2d2d2));

这个属性在按钮制作中用的比较少，最主要的原因是，不能很好地跟边框搭配使用。因为新技术的使用需要掌控一个平衡，如果同时使用CSS3做圆角和渐变背景，对于不支持CSS3的浏览器也就是IE用户来说是很大的伤害。