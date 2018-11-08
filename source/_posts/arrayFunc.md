---
title: 数组操作&字符串
date: 2017-08-28 14:18:47
categories: 前端
tags: [js]
description: js 数组详细操作方法及解析合集&字符串
---

# js 数组详细操作方法及解析合集

## Array.of 
### 返回由所有参数值组成的数组，如果没有参数，就返回一个空数组
```
    let a = Array.of(3, 11, 8); // [3,11,8]
    let a = Array.of(3); // [3]
```

## Arrary.from()

```
 // 1. 对象拥有length属性
let obj = {0: 'a', 1: 'b', 2:'c', length: 3};
let arr = Array.from(obj); // ['a','b','c'];
// 2. 部署了 Iterator接口的数据结构 比如:字符串、Set、NodeList对象
let arr = Array.from('hello'); // ['h','e','l','l','o']
let arr = Array.from(new Set(['a','b'])); // ['a','b']
```

# 比较array，string截取方法
1. string(substr, slice)都__不会改变原来的字符串__,返回的都是被截取的字符串,都可以负数。__substr(index,length)__;__slice(start,end)__
2. array(splice, slice)splice会改变原来数组，slice不会，返回的都是被截取的数组,都可以负数。__splice添加是在开始的元素前面添加的__,eg如下，__splice(index, length, item1,...,itemx)__; __slice(start, end)__
3. 共同点默认都是截取到最后面，都可以由负数开始,，都不可以从后面截取到前面，如果超过，就截取到最后一个。__substr~=splice__
```
let b = [1, 2, 3, 4, 5, 6, 7];
let item = b.splice(-1,0,'添加1','添加2'); 
// [] 没有删除元素，返回空数组
console.log(b); 
// [1,2,3,4,5,6,'添加1','添加2',7] 在最后一个元素的前面添加两个元素
```

# array ,string共有方法（indexOf, slice）


# Array三类操作方法
### 改变原数组的值
```
let a = [1,2,3];
a.splice()
a.copyWithin() //用不多
a.sort() //return<0,不变，等于0，不变，大于0，变
a.push() //加在最后
a.unshift() //加在第一个
a.pop() //减去最后
a.shift() //减去第一个
a.reverse()//掉转

a.fill()

第一个元素(必须): 要填充数组的值
第二个元素(可选): 填充的开始位置,默认值为0
第三个元素(可选)：填充的结束位置，默认是为this.length
['a', 'b', 'c'].fill(7) // [7, 7, 7]
['a', 'b', 'c'].fill(7, 1, 2) // ['a', 7, 'c']
```
### 不会改变原数组（）
```
a.slice();
a.join()
a.cancat()
a.toLocateString()
a.toStrigin()
a.indexOf()
a.lastIndexOf()
a.includes()
```
3. 数组的遍历方法（）

array.keys()
array.values()
array.entries()

```
for (let index of ['a', 'b'].keys()) {
    console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
    console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
    console.log(index, elem);
}
// 0 "a"
// 1 "b"
```

### 相对于的Object.keys(obj);
```
var obj = {
    xin: 'pang',
    name: 'zhicong'
}
var entries = Object.entries(obj);
var keys = Object.keys(obj);
var values = Object.values(obj);
console.log(arr);
```


__参考文档__  
[掘金链接https://juejin.im/post/5b0903b26fb9a07a9d70c7e0](https://juejin.im/post/5b0903b26fb9a07a9d70c7e0)



