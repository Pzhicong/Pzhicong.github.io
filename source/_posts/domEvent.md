---
title: 网页滚动事件监听
date: 2017-08-26 15:07:04
tags: [js]
---

# 测试一下各大主流浏览器下window和document以及document.body上的滚动事件

    几乎所有浏览器都不支持在document.body上监听整个网页的滚动事件，除了QQ浏览器
    几乎所有浏览器都支持在window对象上监听整个网页的滚动事件
    几乎所有浏览器都支持在document.documentElement对象上监听整个网页的滚动事件，除了QQ浏览器

# 测试一下各大主流浏览器下window和document以及document.body上的scrollTop属性

1. 几乎所有浏览器都支持用document.documentElement.scrollTop来获取网页的滚动高度,除了Chrome和Safari
2. 只有Chrome和Safari支持用document.body.scrollTop来获取网页的高度

# 最佳实践
1. 把获取滚动高度的事件处理程序绑定到window对象上window.addEventListener('scroll', func);
2. `网页的真实滚动高度` Math.max(document.body.scrollTop, document.documentElement.scrollTop)

# 滚动到某处
  `window.scrollTo(x, y)`;