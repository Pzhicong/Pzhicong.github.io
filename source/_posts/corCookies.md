---
title: koa2跨域设置cookies
date: 2018-04-29 13:34:32
categories: 前端
tags: [node]
description: 当我们需要跨域设置cookies的时候。一般的，是设置不进去的，浏览器即使接受到setCookies请求，但实际上没有设置进去，所以在其他请求的时候，也不会带上。这时候，就需要我们设置一下请求参数了。
---

# 当我们需要跨域设置cookies的时候。一般的，是设置不进去的，浏览器即使接受到setCookies请求，但实际上没有设置进去，所以在其他请求的时候，也不会带上。这时候，就需要我们设置一下请求参数了。

1. 前端需要xhr.withCredentials = true 来标识传输cookie
2. 服务端配置 需要设置 Access-Control-Allow-Credentials：true 才能获得前端的cookie

### 前端
```
$.ajax({
    url: 'http://10.50.101.79:3000/o2o-web/orderInfoCache',
    data: data4,
    type: 'POST',
    crossDomain: true, 
    xhrFields: {withCredentials: true},
    success: function (data) {
        console.log(data)
    },
    error: function (err) {
        console.log(err)
    }
})
```

### 后台

```
app.use(cors({
  // origin: 'http://weixin.mama100.cn',
  credentials: true
}));
```