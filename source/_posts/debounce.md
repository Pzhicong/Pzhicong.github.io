---
title: 防抖与节流
date: 2017-04-26 15:52:32
categories: 前端
tags: [js]
description: 窗口的resize、scroll、输入框内容校验等操作时，采用debounce（防抖）和throttle（节流）的方式来减少触发的频率。
---

# 防抖动（debouce）
## 若干个函数调用合成 一次，并在给定时间过去之后仅被调用一次

```
// immediate是否理解执行
function debouce(func,delay,immediate){
    var timer = null;
    return function(){
        var context = this;
        var args = arguments;
        if(timer) clearTimeout(time);
        if(immediate){
            //根据距离上次触发操作的时间是否到达delay来决定是否要现在执行函数
            var doNow = !timer;
            //每一次都重新设置timer，就是要保证每一次执行的至少delay秒后才可以执行
            timer = setTimeout(function(){
                timer = null;
            },delay);
            //立即执行
            if(doNow){
                func.apply(context,args);
            }
        }else{
            timer = setTimeout(function(){
                func.apply(context,args);
            },delay);
        }
    }
}

function foo() {
  console.log('You are scrolling!');
}

// 在 debounce 中包装我们的函数，过 2 秒触发一次
window.addEventListener('scroll', debounce(foo, 2000, true));

```

# 节流（throttle）
## 节流函数允许一个函数在规定的时间内只执行一次

```
/**
 * @desc 函数节流
 * @param func 函数
 * @param wait 延迟执行毫秒数
 * @param type 1 表时间戳版，2 表定时器版
 */
function throttle(func, wait ,type) {
    if(type===1){
        var previous = 0;
    }else if(type===2){
        var timeout;
    }

    return function() {
        var context = this;
        var args = arguments;
        if(type===1){
            var now = Date.now();

            if (now - previous > wait) {
                func.apply(context, args);
                previous = now;
            }
        }else if(type===2){
            if (!timeout) {
                timeout = setTimeout(function() {
                    timeout = null;
                    func.apply(context, args)
                }, wait)
            }
        }

    }
}

```