---
title: marginover问题
date: 2016-05-29 10:41:52
tags: [css]
description: div嵌套引起的margin-top不起作用。
---

## 嵌套div中margin-top转移问题的解决办法


>在这两个浏览器中，有两个嵌套关系的div，如果外层div的父元素padding值为0，那么内层div的margin-top或者margin-bottom的值会“转移”给外层div。

>原因：盒子没有获得 haslayout  造成 margin-top无效

解决办法：
1、在父层div加上：overflow:hidden；
2、把margin-top外边距改成padding-top内边距 ；
3、父元素产生边距重叠的边有不为 0 的 padding 或宽度不为 0 且 style 不为 none 的 border。父层div加： padding-top: 1px;
4、让父元素生成一个 block formating context，以下属性可以实现
>float: left/right
>position: absolute
>display: inline-block/table-cell(或其他 table 类型)
>overflow: hidden/auto
>父层div加：position: absolute;

## 实例

<iframe width="100%" height="300" src="//jsrun.net/jAZKp/embedded/all/light/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>