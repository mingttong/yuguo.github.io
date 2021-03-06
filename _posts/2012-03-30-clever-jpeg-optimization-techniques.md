---
layout: post
title: 关于JPEG压缩优化的一点趣事
date: 2012-03-30 18:37
comments: true
categories: [front-end]
---

今天看了一篇老文，对JPEG的压缩优化有了更深刻的理解，对JPEG存储数据的方法也更加了解。

原文地址：<a href="http://www.smashingmagazine.com/2009/07/01/clever-jpeg-optimization-techniques/">http://www.smashingmagazine.com/2009/07/01/clever-jpeg-optimization-techniques/</a><h2>8x8像素格</h2><a href="http://yuguo.us/files/2012/03/grid-block1.png"><img class="aligncenter size-full wp-image-1151" title="grid-block" src="http://yuguo.us/files/2012/03/grid-block1.png" alt=""   data-pinit="registered" /></a>
这是一个黑白格的图片，在保存为JPEG的时候选择“质量”很低的话，就会出现模糊的锯齿。这种锯齿我们经常见到，但神奇的是左上角的那个正方形的边缘非常完美，这是因为JPEG储存图片的时候会把图片存为一系列8x8像素的块。

左上角的图片正好压在8x8的边缘，所以显示很完美，右下角的图片没有压在8x8上，所以会会模糊的多。

所以优化技巧就是，<strong>如果图片包含轮廓清晰的元素，那么就把元素挪到8x8的参考线上</strong>。

在PS里设置网格的尺寸的方法是编辑&gt;首选项&gt;参考线、网格和切片&gt;网格&gt;网格线间隔设置为8的倍数比如32，然后子网格设置为4（32/8=4）即可。

显示网格的方法是视图&gt;显示&gt;网格。
<h2>颜色优化</h2>
颜色优化的具体处理方法有点复杂，建议阅读原文，按原文步骤操作一遍就大概明白了。

核心思想就是把图片模式设置为<strong>lab颜色</strong>的话，会在通道中看见图片被分成明度、a、b三个通道。跟RGB模式有3个通道分别储存红色绿色蓝色不一样的是，lab颜色模式中明度通道储存某一点的明亮程度，a通道储存某一点的红色和绿色有多少，b通道储存蓝色和黄色有多少。
<a href="http://yuguo.us/files/2012/03/2.jpg"><img class="aligncenter size-full wp-image-1152" title="2" src="http://yuguo.us/files/2012/03/2.jpg" alt=""   data-pinit="registered" /></a>
那么图片中有一些黑色的部分的话，那么就会在明度通道中有暗度信息表明这一点非常之暗，同时又会在红绿通道中储存信息表明这一点很红。这从人眼可视的角度来看就是冗余信息，因为<strong>很暗的黑色和很暗的红色对于人眼都是黑色</strong>。

知道了这一点以后，我们来操作a通道（或者b通道，看具体的图中黑色周围是什么色调了），选取了a通道之后把黑色部分的红绿信息抹掉，抹掉的方法就很多了，比如滤镜&gt;杂色&gt;中间值，会把红黑混杂的颜色改成中间色。这样就能减少一些文件体积。

如果你是变态的对色温极端追求的设计师，而且显示器有非常之好的话，那么你会发现颜色可能会偏暗了，这时候对于原图调一下色阶就好了。Ctrl+L，照片处理中常用的快捷键。
<h2>通用规则</h2>
1.程序是死的，人是活的。程序无法判断压缩jpeg多少是一个合适的比例，但人可以。

2.永远不要把jpeg保存为100%质量，95%是你的上限，如果你需要很高的质量的话。

3.如果你的图片中有一些尖锐的元素或者边缘，那么不要设置低于51%的质量。

4.还有一种格式叫png，合理使用它（png的优化规则同样非常复杂）。

