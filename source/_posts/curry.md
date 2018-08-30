---
title: 柯里化
date: 2016-08-28 09:20:08
tags: [js]
description: 函数柯里化指的是将能够接收多个参数的函数转化为接收单一参数的函数，并且返回接收余下参数且返回结果的新函数的技术。
---

# 函数柯里化指的是将能够接收多个参数的函数转化为接收单一参数的函数，并且返回接收余下参数且返回结果的新函数的技术。

```
function multi() {
    var args = Array.prototype.slice.call(arguments);
	var fn = function() {
		var newArgs = args.concat(Array.prototype.slice.call(arguments));
        return multi.apply(this, newArgs);
    }
    fn.toString = function() {
        return args.reduce(function(a, b) {
            return a * b;
        },1)
    }
    return fn;
}
//multi(2)(3)(4)
//multi(2, 3)(4)
```