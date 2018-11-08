---
title: tspratice
date: 2018-10-23 10:28:13
tags:
description: ts学习
---

##数组
```
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```

##类型断言
```
let strLength: number = (<string>someValue).length;
let strLength: number = (someValue as string).length;
```

##结构
```
//对象
var obj = { a: 'a', b: 'b' };
var { a,...defaultobj } = obj;

//数组
let first = [1, 2];
let second = [3, 4];
let bothPlus = [0, ...first, ...second, 5];

对象展开还有其它一些意想不到的限制。 首先，它仅包含对象 自身的可枚举属性。 大体上是说当你展开一个对象实例时，你会丢失其方法：
class C {
  p = 12;
  m() {
  }
}
let c = new C();
let clone = { ...c };
clone.p; // ok
clone.m(); // error!
```

##接口
```
interface Point {
    readonly x: number;
    readonly y: number;
}
let p1: Point = { x: 10, y: 20 };
p1.x = 5; // error!

let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error!
a = ro as number[];//true

//额外属性检查
interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}

//方法
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}
```

### 泛型

