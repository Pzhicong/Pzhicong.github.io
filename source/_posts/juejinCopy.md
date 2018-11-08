---
title: 掘金复制方案
date: 2016-08-28 15:11:02
categories: 前端
tags: [js]
description: 玩掘金、知乎的时候复制一段文字，总是会在内容后面加上一些版权信息,这到底是怎么实现的呢
---

# 玩掘金、知乎的时候复制一段文字，总是会在内容后面加上一些版权信息,这到底是怎么实现的呢（直接撸码）

```
// 掘金这里不是全局监听，应该只是监听文章的dom范围内。

document.body.oncopy = event => {
    event.preventDefault(); // 取消默认的复制事件
    let textFont, copyFont = window.getSelection(0).toString(); // 被复制的文字 等下插入
    // 防知乎掘金 复制一两个字则不添加版权信息 超过一定长度的文字 就添加版权信息
    if (copyFont.length > 10) {
        textFont = copyFont + '\n' +
            '作者：OBKoro1\n' +
            '链接：https://juejin.im/user/58714f0eb123db4a2eb95372/posts\n' +
            '来源：掘金\n' +
            '著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。';
    } else {
        textFont = copyFont; // 没超过十个字 则采用被复制的内容。
    }
    if (event.clipboardData) {
        return event.clipboardData.setData('text', textFont); // 将信息写入粘贴板
    } else {
        // 兼容IE
        return window.clipboardData.setData("text", textFont);
    }
}
```

# 实现类起点网的防复制功能:
1. 禁止复制+剪切
2. 禁止右键，右键某些选项:全选，复制，粘贴等。
3. 禁用文字选择，能选择却不能复制，体验很差。
4. user-select 用css禁止选择文本。

```
// 禁止右键菜单
//使用e.preventDefault()也可以禁用，但建议使用return false这样就不用去访问e和e的方法了。
document.body.oncontextmenu = e => {
    console.log(e, '右键');
    return false;
    // e.preventDefault();
};

// 禁止文字选择。

document.body.onselectstart = e => {
    console.log(e, '文字选择');
    return false;
    // e.preventDefault();
};

// 禁止复制

document.body.oncopy = e => {
    console.log(e, 'copy');
    return false;
    // e.preventDefault();
}

// 禁止剪切
document.body.oncut = e => {
    console.log(e, 'cut');
    return false;
    // e.preventDefault();
};

// 禁止粘贴
document.body.onpaste = e => {
    console.log(e, 'paste');
    return false;
    // e.preventDefault();
};

// css 禁止文本选择 这样不会触发js
body {
    user - select: none; -
    moz - user - select: none; -
    webkit - user - select: none; -
    ms - user - select: none;
}

//复制
function copyText() {
 var text = document.getElementById("text").innerText; // 获取要复制的内容也可以传进来
 var input = document.getElementById("input"); // 获取隐藏input的dom
 input.value = text; // 修改文本框的内容
 input.select(); // 选中文本
 document.execCommand("copy"); // 执行浏览器复制命令
 alert("复制成功");
}

```


