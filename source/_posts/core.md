---
title: core
date: 2017-09-14 15:19:42
tags: [browser]
categories: [前端]
# description: 当我们需要跨域设置cookies的时候。
---

# 浏览器内核简介

## webkit自从__Apple开发源码__之后，经过__google大力发展__，目前webkit已经相当盛行

国内常见的浏览器有：IE、Firefox、QQ浏览器、Safari、Opera、Google Chrome、百度浏览器、搜狗浏览器、猎豹浏览器、360浏览器、UC浏览器、遨游浏览器、世界之窗浏览器等。但目前最为主流浏览器有五大款，分别是IE、Firefox、Google Chrome、Safari、Opera。 

国内常见的浏览器有：IE、Firefox、QQ浏览器、Safari、Opera、Google Chrome、百度浏览器、搜狗浏览器、猎豹浏览器、360浏览器、UC浏览器、遨游浏览器、世界之窗浏览器等。但目前最为主流浏览器有五大款，分别是IE、Firefox、Google Chrome、Safari、Opera。 

微信浏览器（X5内核）被前端届称为移动端的“IE6”。最近发现自己的一个WebApp在微信下面出现一个坑爹的问题，所以想写一篇文章来总结一下自己在微信开发中所遇到的一些问题和解决办法，给自己和其它人提供参考。

先说一下微信浏览器的情况，__安卓端微信6.1版本以上使用的是QQ浏览器X5的内核，5.4~6.1之间如果用户手机上安装过QQ浏览器，则使用X5内核。如果未安装，则使用系统浏览器内核__

__iOS端的情况比较简单，由于苹果的限制，微信只能使用Safari提供的内核(也就是Webkit)。相对来说iOS的问题不多，问题主要集中在安卓端，也就是X5内核上面。__

五大浏览器：
1. 五大浏览器
2. Opera浏览器
3. Safari浏览器
4. Firefox浏览器
5. Chrome浏览器

四大内核分别
1. Trident（也称IE内核）
2. webkit
3. Blink
4. Gecko

作为前端开发，熟悉四大内核是非常有必要的。四大内核的解析不同使网页渲染效果更具多样化。下面总结一下各常用浏览器所使用的内核。

1. __IE浏览器内核：Trident内核，也是俗称的IE内核；__
2. __Chrome浏览器内核：统称为Chromium内核或Chrome内核，以前是Webkit内核，现在是Blink内核；__
3. __Firefox浏览器内核：Gecko内核，俗称Firefox内核；__ 
4. __Safari浏览器内核：Webkit内核；__ 
5. __Opera浏览器内核：最初是自己的Presto内核，后来是Webkit，现在是Blink内核；__
6. 360浏览器、猎豹浏览器内核：IE+Chrome双内核； 
7. 搜狗、遨游、QQ浏览器内核：Trident（兼容模式）+Webkit（高速模式）； 
8. 百度浏览器、世界之窗内核：IE内核； 
9. 2345浏览器内核：以前是IE内核，现在也是IE+Chrome双内核；

### css前缀
```
-moz-     /* 火狐等使用(Gecko)Mozilla浏览器引擎的浏览器 */
-webkit-  /* Safari, 谷歌浏览器等使用Webkit引擎的浏览器 */
-o-       /* Opera浏览器(早期) */
-ms-      /* Internet Explorer (不一定) */ 

-webkit-transform:rotate(-3deg); /为Chrome/Safari/ 
-moz-transform:rotate(-3deg); /为Firefox/ 
-ms-transform:rotate(-3deg); /为IE/ 
-o-transform:rotate(-3deg); /为Opera/ 
transform:rotate(-3deg); /为nothing/
```



### 参考文献
[五大主流浏览器及四大内核 - CSDN博客](https://blog.csdn.net/yuyanjing123456789/article/details/78689595)
