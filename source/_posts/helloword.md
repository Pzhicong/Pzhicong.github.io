---
title: 使用Hexo搭建个人博客
layout: post
date: 2016-05-03 19:07:43
comments: true
categories: 前端
tags: [Hexo]
keywords: Hexo, Blog
description: 生命在于折腾，又把博客折腾到Hexo了。给Hexo点赞。
---

## 常用命令


``` bash
hexo new "My New Post" || hexo new post "postName"
hexo s --debug
hexo clean
hexo d -g
```

## 重装之痛
#### 我是根据这个来建站的 
[建站](https://www.cnblogs.com/fengxiongZz/p/7707219.html)
[文档](https://hexo.io/zh-cn/docs/index.html)

## FOR个人（仔细看建站链接）
    1.下载github的代码
    2.切换远程分支 (git checkout -b release origin/release)
    3.全局安装hexo (npm install hexo -g)（可能需要npm install hexo-deployer-git --save）
    4.在目录下安装 npm install
    5.记得把自己已改过的next主题下载下来，覆盖本地的
    6.有可能git有问题，貌似说没配置path变量，可以试试
    7.all is ok

## setting sync配置 [根据](https://juejin.im/post/5a08d1d6f265da430f31950e)
    1.F1,sync reset extent
    2.先ctrl+shift+D,按需要填token，giftID
    3.修改配置，不能直接同步这些配置（比如颜色主题之类的）
