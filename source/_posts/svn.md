---
title: svn
date: 2016-05-18 10:54:27
categories: 前端
tags: [tools]
description: svn更新，清理时出现乱码解决方案。
---

### svn 更新失败

#### [参考博客](https://blog.csdn.net/allen_a/article/details/51330739)

1. 下载 sqlite数据库工具，sqlite3.exe下载地址：sqlite官网[链接](http://www.sqlite.org/download.html),我这里是windows操作系统，因此下载 Precompiled Binaries for Windows版本的压缩包即可(ps:现在下载的都是sqlite.dll,所以使用不了了，我已经放到自己百度云,[链接](https://pan.baidu.com/s/15w9SKYL_9hUVDncJOR_TzA))

2. 将下载到的 sqlite3.exe文件复制到本地磁盘的某个目录下,然后找到本地svn文件库下.svn/wc.db文件， 将其复制到该目录下

3. 点击开始 -> 运行 -> cmd，打开cmd窗口，输入以下命令：
   sqlite3 wc.db
   select* from work_queue;
   如果此时查询有记录，则执行以下命令：
   delete from work_queue;

4. 将wc.db文件，覆盖本地svn文件库目录 .svn目录下的wc.db文件

5. 然后再右键点击本地svn文件库目录,执行clean up,就能够正常清理了。(如果清理失败的话，请勾选“解锁”)
![clean](/images/svn/clean.png)