---
title: redux
date: 2018-01-27 18:48:38
tags: [js]
---

# redux学习记录

## 三个对象 __state__,__action__,__reducer__

1. store.getState()
2. action 一个对象
3. Reducer 只是一些纯函数，它接收先前的 state 和 action，并返回新的 state
4. 不直接修改 state 中的字段，而是返回新对象