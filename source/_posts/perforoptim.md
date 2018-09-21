---
title: 性能优化
date: 2018-09-06 10:36:02
tags: [chrome]
categories: [前端]
---

## 史上最强的性能优化方案

#### webp格式图片兼容写法
```
//如果浏览器支持WebP格式，就会加载image.webp，否则会加载image.jpg
<picture class="picture">
  <source type="image/webp" srcset="image.webp">
  <img class="image" src="image.jpg">
</picture>
```

__DOM__
1. 在JS中对DOM进行访问的代价非常高。请尽可能减少访问DOM的次数（建议缓存DOM属性和元素、把DOM集合的长度缓存到变量中并在迭代中使用。读变量比读DOM的速度要快很多。）
2. 重排与重绘的代价非常昂贵。如果操作需要进行多次重排与重绘，建议先让元素脱离文档流，处理完毕后再让元素回归文档流，这样浏览器只会进行两次重排与重绘（脱离时和回归时）。
3. 善于使用事件委托

__图片优化__
1. 尽可能通过srcset，sizes和<picture>元素使用响应式图片。还可以通过<picture>元素使用WebP格式的图像。
2. 使用自动播放与循环的HTML5视频替换GIF图，因为视频比GIF文件还小（好消息是未来可以通过img标签加载视频）

__defer__
使用defer或async。如果使用defer或async请将Script标签放到head标签中，__以便让浏览器更早地发现资源并在后台线程中解析并开始加载JS__。

__使用Intersection Observer实现懒加载__ [兼容性](https://caniuse.com/#search=Intersection)

1. 可以通过Intersection Observer延迟加载图片、视频、广告脚本、或任何其他资源。
2. 可以先加载低质量或模糊的图片，当图片加载完毕后再使用完整版图片替换它。
3. 延迟加载所有体积较大的组件、字体、JS、视频或Iframe是一个好主意

__优先加载关键的CSS__
CSS资源的加载对浏览器渲染的影响很大，默认情况下浏览器只有在完成<head>标签中CSS的加载与解析之后才会渲染页面。如果CSS文件过大，用户就需要等待很长的时间才能看到渲染结果。针对这种情况可以将首屏渲染必须用到的CSS提取出来内嵌到<head>中，然后再将剩余部分的CSS用异步的方式加载。可以通过Critical做到这一点。
