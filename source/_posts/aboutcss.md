---
title: 常用css
date: 2018-05-17 14:07:25
categories: 前端
tags: [css]
description: css记录
---

## CSS中一个冒号和两个冒号有什么区别  [链接](https://shimo.im/doc/y18XXPd7hicGHr2H/) [链接](http://www.alloyteam.com/2016/05/summary-of-pseudo-classes-and-pseudo-elements/) [important](https://blog.csdn.net/qq_25292481/article/details/52577320)

我们来说说什么是伪类。
根据w3c，CSS 伪类用于向某些选择器添加特殊的效果。最常见的就是对a元素的一些伪类了，比如a:linked,a:visted,a:hover,a:active等等。
再说说伪元素。根据w3c，CSS 伪元素用于将特殊的效果添加到某些选择器。比如p:before，p:after，p:first-letter，p:first-line等等。当然有时候我们可能会看到这样的写法也能正确执行：p::before，p::after，p::first-letter，p::first-line,为什么？因为在css3以前对伪类及伪元素的写法都只是单冒号，而css3为了更好的区分开伪类及伪元素就规定对伪元素使用双冒号的写法，显然这个出发点是好的，但是为了兼容不支持css3这种特性的浏览器，我们还是先老实的用单冒号的写法吧。

![rect](/images/伪类.png)
![rect](/images/伪元素.png)

### 伪类
```
//常见伪类
:first-child,:link,:vistited,:hover,:active:focus,:lang,:not(s),：root等...
```
<iframe width="100%" height="300" src="//jsrun.net/kDZKp/embedded/all/light/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

#### :first-child :last-child .demo li:not(:last-child) :first-of-type p:last-of-type
p:first-child 指的是P元素，并且P元素是其父元素的第一个子元素，last-child也是
p:first-of-type 选择每个p元素是其父级的第一个p元素

p:nth-child(2) 选择每个p元素是其父级的第二个子元素,odd:单数，even:双数  (nth-last-child(2)  从最后一个子项计数)
p:nth-of-type(2) 选择每个p元素是其父级的第二个p元素 (p:nth-last-of-type(2) 从最后一个子项计数)
### 伪元素
```
//常见伪元素
:first-line，:first-letter，:before，:after，:placeholder,:selection
```

#### ::first-letter ::first-line ::selection
<iframe width="100%" height="300" src="//jsrun.net/ZDZKp/embedded/all/light/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
