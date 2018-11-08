---
title: httpcache
date: 2018-08-31 11:07:19
categories: 前端
tags: [css]
# description: css记录
---

## 缓存机制无处不在，有客户端缓存，服务端缓存，代理服务器缓存等。在HTTP中具有缓存功能的是浏览器缓存。

## 我们将缓存分为强缓存和弱缓存
![clean](/images/cache/1.jpg)

浏览器请求一个静态资源时的HTTP流程

![clean](/images/cache/2.jpg)

### 强缓存与弱缓存的区别：
获取资源形式： 都是从缓存中获取资源的。
状态码： 强缓存返回200(from cache),弱缓存返回304状态码
强缓存不发送请求，直接从缓存中取。
弱缓存需要发送一个请求，验证这个文件是否可以使用（有没有被改动过）。

### 强缓存  
1. Cache-Control（__http1.1__）
2. Expires (__http1.0__)

Exprires的值为服务端返回的数据到期时间。当再次请求时的请求时间小于返回的此时间，则直接使用缓存数据。但由于服务端时间和客户端时间可能有误差（浏览器端可以随意修改时间），这也将导致缓存命中的误差，另一方面，Expires是HTTP1.0的产物，故现在大多数使用Cache-Control替代。当Expires和Cache-Control同时存在时，浏览器先检查 Cache-Control，如果有，则以 Cache-Control 为准，忽略 Expires。如果没有 Cache-Control，则以 Expires 为准。

### Cache-ControlCache-Control有很多属性，不同的属性代表的意义也不同。
1. private：客户端可以缓存 （浏览器请求服务器时，中间服务器都要把浏览器的请求透传给服务器）
2. public：客户端和代理服务器都可以缓存（浏览器请求服务器时，如果缓存时间没到，中间服务器直接返回给浏览器内容，而不必请求源服务器。）
3. max-age=t：缓存内容将在t秒后失效
3. no-cache：需要使用协商缓存来验证缓存数据（浏览器不做缓存检查）
4. no-store：所有内容都不会缓存。



### 弱缓存
Last-Modified & if-modified-since（__http1.0__,前者是response，后者request）
ETag & If-None-Match（__http1.1__,前者是response，后者request）

### Etag 主要为了解决 Last-Modified 无法解决的一些问题：
1. 一些文件也许内容并不改变(仅仅改变的修改时间)，这个时候我们不希望文件重新加载。（Etag值会触发缓存，Last-Modified不会触发）
2. If-Modified-Since能检查到的粒度是秒级的，当修改非常频繁时，Last-Modified会触发缓存，而Etag的值不会触发，重新加载。
3. 某些服务器不能精确的得到文件的最后修改时间

#### 但是实际应用中由于Etag的计算是使用算法来得出的，而算法会占用服务端计算的资源，所有服务端的资源都是宝贵的，所以就很少使用Etag了。
#### 同时使用这两个报文头（__Last-Modified__ && __ETag__），两个都匹配才会命中弱缓存，否则将重新请求资源。

### 不同刷新的请求执行过程
1. 浏览器地址栏中写入URL，回车浏览器发现缓存中有这个文件了，不用继续请求了，直接去缓存拿。（最快）
2. F5就是告诉浏览器，别偷懒，好歹去服务器看看这个文件是否有过期了（强缓存）。于是浏览器就胆胆襟襟的发送一个请求带上If-Modify-since（弱缓存）。
3. Ctrl+F5告诉浏览器，你先把你缓存中的这个文件给我删了，然后再去服务器请求个完整的资源文件下来。于是客户端就完成了强行更新的操作.


## 最终方案
#### 痛点
浏览器无法主动得知服务器上的 a.js 资源变化了。
不管用 Expires 还是 Cache-Control，他们都只能够控制缓存是否过期，但是在缓存过期之前，浏览器是无法得知服务器上的资源是否变化的。只有当缓存过期后，浏览器才会发请求询问服务器。
大家可以想象我们使用 a.js 的场景，我们一般都是输入网址，访问一个 html 文件，html文件中会引入 js、css
、图片等资源。
所以呢，我们在html上做些手脚。
我们不让 html 文件缓存，每次访问 html 都去请求服务器。所以浏览器每次都能拿到最新的html资源。
a.js 内容更新的时候，我们修改一下 html 中 a.js 的版本号。
第一次访问 html
```
<script src="http://test.com/a.js?version=0.0.1"></script>
```
浏览器下载0.0.1版本的a.js文件。
浏览器再次访问 html，发现还是0.0.1版本的a.js文件，则使用本地缓存。
某一天a.js变了，我们的html文件也相应变化如下：
```
<script src="http://test.com/a.js?version=0.0.2"></script>
```
浏览器再次访问html，发现【test.com/a.js?versio… a.js。
如此往复。
所以，通过设置html不缓存，html引用资源内容变化则改变资源路径的方式，就解决了无法及时得知资源更新的问题。
当然除了以版本号来区分，也可以以 MD5hash 值来区分。 如

<script src="http://test.com/a.【hash值】.js"></script>
复制代码
使用webpack打包的话，借助插件可以很方便的处理。

### 缓存的优先级
强制缓存的优先级高于协商缓存，Pragma的优先级高于Cache-Control，那么其他缓存的优先级顺序怎么样呢？
```
Pragma > Cache-Control > Expires > ETag > Last-Modified
```

Cache-Control 
no-cache — 强制每次请求直接发送给源服务器，而不经过本地缓存版本的校验。这对于需要确认认证应用很有用（可以和public结合使用），或者严格要求使用最新数据 的应用（不惜牺牲使用缓存的所有好处） 
Pragma 当”no-cache”出现在请求消息中时，应用程序应当向原始服务器推送此请求，即使它已 
经在上次请求时已经缓存了一份拷贝。这样将保证客户端能接收到最权威的回应。它也用来在客户端发现其缓存中拷贝不可用或过期时，对拷贝进行强制刷新。

cache-control 
max-age>0 时 __直接从游览器缓存中 提取__
max-age<=0 时 __向server 发送http 请求确认 ,该资源是否有修改__
有的话 返回200 ,无的话 返回304.


#### 参考文旦
[node实践前端也要懂Http缓存机制 - 掘金](https://juejin.im/post/5b70edd4f265da27df0938bc)
[面试精选之http缓存 - 掘金](https://juejin.im/post/5b3c87386fb9a04f9a5cb037)
[HTTP----HTTP缓存机制 - 掘金](https://juejin.im/post/5a1d4e546fb9a0450f21af23)
[浅析HTTP缓存的机制-浏览器缓存 | OBKoro1's Blog](http://obkoro1.com/2018/06/09/%E6%B5%85%E6%9E%90HTTP%E7%BC%93%E5%AD%98%E7%9A%84%E6%9C%BA%E5%88%B6-%E6%B5%8F%E8%A7%88%E5%99%A8%E7%BC%93%E5%AD%98/)
