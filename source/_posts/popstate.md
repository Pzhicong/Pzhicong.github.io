---
title: popstate
date: 2017-06-05 10:41:29
tags: [js]
---

# popstate玩转浏览器历史记录

### HTML5的新API扩展了window.history，在过去，window.history对象，该对象上包含有length和state的两个值，在它的__proto__上继承有__back、forward、go__等几个功能函数，在popstate之前，我们可以利用back、forward、go对history进行后退和前进操作。你可以使用go()方法从当前会话的历史记录中加载页面（当前页面位置索引值为0，上一页就是-1，下一页为1）
__弊端__：只能操作前进后退，但是无法控制前进后要去哪，history.length都只会维持原来的状态

### popstates使用
history.pushState(data,title,url);
replaceState用法类似，例如：history.replaceState("首页","",location.href+ "#news");
//其中第一个参数data(可以是obj)是给state的值；第二个参数title为页面的标题，但当前所有浏览器都忽略这个参数，传个空字符串就好；第三个参数url是你想要去的链接(同域名),__返回的时候，或前进时并不会重新加载这个URL页面__；

__两者区别__：pushState会改变history.length，而replaceState不改变history.length

### 监听历史记录点

监听历史记录点，直观的可认为是监听URL的变化，但会忽略URL的hash部分，监听URL的hash部分，HTML5有新的API为onhashchange，我的博客里也有说到该方法和跨浏览器的兼容解决方案。可以通过window.onpopstate来监听url的变化，并且可以获取存储在该历史记录点的状态对象，也就是上文说到的json对象，如：

```
window.onpopstate=function()
{
    var json=window.history.state;
}

window.addEventListener("popstate", function () {
        console.log(history.state);
    },false);
```

## 禁止返回上一页
```
history.pushState(null, null, document.URL);
window.addEventListener("popstate",function(e) {  
  console.log(e);
  history.pushState(null, null, document.URL);
}, false);
```
这个其实就是利用pushState向浏览历史列表中插入当前页面，在点击后退前和点击时都插入一次，那样无论点前进还是后退永远都会留在这个页面了

__值得注意的是：javascript脚本执行window.history.pushState和window.history.replaceState不会触发onpopstate事件,a标签，或其它会如go，back,forward。__

还有一点注意的是，谷歌浏览器和火狐浏览器在页面第一次打开的反应是不同的，谷歌浏览器奇怪的是回触发onpopstate事件，而火狐浏览器则不会。


```
window.onbeforeunload = function (e) {
    if (e) {
        e.returnValue = '关闭提示';
    }
    // Chrome, Safari, Firefox 4+, Opera 12+ , IE 9+
    return '关闭提示';
}
```

__从2011年5月25日起,  HTML5 规范 声明:在该事件的处理函数中调用下列弹窗相关的方法时,可以忽略不执行,window.showModalDialog(), window.alert(), window.confirm() window.prompt().__

### 解决问题（ios"往返缓存"）
在微信浏览器中，从一个html跳到另外一个html页面后，点击浏览器自带的历史返回按钮，或者在第二个页面中调用history.back()等返回上一页方法，在安卓中直接会返回上一页（相当于重新加载上一页所有内容和逻辑，js会重新在执行一遍），但是在苹果端中，返回到上一页是，浏览器会读取缓存中的页面内容（回到上一页，会保持跳转时的状态，包括刚才浏览的位置，但是js不会在重新执行，因为是直接读取缓存中的），这种缓存被称作“往返缓存”，是为了提高用户在触发“返回”或者“前进”时页面的响应速度。这个缓存不仅保存页面所有数据，还保存着页面DOM和JS的状态，如果页面处于“往返缓存”中，在次进入这个页面将不会触发 onload 事件。

苹果系统的这种默认行为对于用户浏览列表，点击某个单个产品，查看详情，在返回上一页列表页的情况下，表现较好，因为会保持状态，默认回到上一个列表页的位置，但是在需要根据接口返回code或者value去判断某种状态的时候会产生很多问题，因为js不会重新执行，所有判断条件也就无法执行。所有当页面被保存在“往返缓存”中，时，我们可以想办法去不让浏览器从缓存中读取页面，而是重新加载页面内容

__方法一__
```
window.addEventListener("popstate", function (e) {
      //检测到用户点击浏览器返回按钮，进行操作
       console.log(document.referrer);

       //使用href的形式去用跳转的形式，跳转到上一页
       document.location.href = document.referrer;
 }, false);
 var state = {
    title: "",
    url: ""
 };
 window.history.pushState(state, "", "");
```

__方法二(亲测有效)__
#### onpageshow事件（这个事件在页面显示时触发，如果页面不在“往返缓存”中，该事件将会在onload后触发） 
在onpageshow事件中，可以利用event.persisted
```
window.addEventListener('pageshow', function(event) {
    alert(event.persisted);
    if (event.persisted) location.reload();    //如果检测到页面是从“往返缓存”中读取的，刷新页面
});
```

__方法三__
#### 指定了 onunload 事件处理程序的页面会被自动排除在 “往返缓存”之外，即使事件 处理程序是空的。原因在于， onunload 最常用于撤销在 onload 中所执行的操作， 而跳过 onload 后再次显示页面很可能就会导致页面不正常
```
window.addEventListener('unload',function () {

})
```


### 参考链接
[图解用HTML5的popstate如何玩转浏览器历史记录 - 水乙 - 博客园](https://www.cnblogs.com/shuiyi/p/5115188.html)