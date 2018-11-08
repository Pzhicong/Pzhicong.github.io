---
title: 移动端布局
date: 2018-11-08 09:47:30
categories: 前端
tags: [css]
---


### 前言
![image](https://pzhicong.github.io/cici/images/youdaoyun/ppi.png)

css px是个相对长度单位，像素（px）是相对于显示器屏幕分辨率而言的国内推荐；
**em**单位名称为**相对长度单位**。相对于当前对象内文本的字体尺寸，国外使用比较多；
像素是一个个点，就是指屏幕能容下多少个点，不是长度单位。主要分为物理像素与逻辑像素，物理像素就是设备的设置的，总共能容下多个个点
，逻辑像素=物理像素/devicePixelRatio，devicePixelRatio也是设备的固定的，
（打开一个页面，移动端浏览器会自动寻找<meta name='viewport'>,如果指定了视窗口的width，就会把页面放到指定width的viewport里面。如果没有指定，则默认值有的是980，具体根据浏览器来定的。（我遇到的就是这个问题，通过添加下面的代码解决））


> (这样整个网页在设备内显示时的页面宽度就会等于设备逻辑像素大小，也就是device-width。这个device-width的计算公式为：
> 设备的物理分辨率/(devicePixelRatio * scale)，在scale为1的情况下，device-width = 设备的物理分辨率/devicePixelRatio 。
> devicePixelRatio称为设备像素比，每款设备的devicePixelRatio都是已知，并且不变的，目前高清屏，普遍都是2，不过还有更高的，比如2.5, 3 等，我魅族note的手机的devicePixelRatio就是3。淘宝触屏版布局的前提就是viewport的scale根据devicePixelRatio动态设置：)

```
<meta name=”viewport” content=”width=device-width, 
    initial-scale=1, maximum-scale=1″>
```

> 先明白几个概念
phys.width:
device-width:
一般我们所指的宽度width即为phys.width，而device-width又称为css-width。
其中我们可以获取phys.width即width通过document.documentElement.clientWidth;而获取css-width通过 window.screen.width获取。如iphone6的phys.width为750px（物理分辨率），而css-width为375px（逻辑分辨率）。[width=device-width作用](https://www.cnblogs.com/wbxjiayou/p/5176815.html) 

- width：控制 viewport 的大小，可以指定的一个值，如果 600，或者特殊的值，如 device-width 为设备的宽度（单位为缩放为 100% 时的 CSS 的像素）。  
- height：和 width 相对应，指定高度。  
- initial-scale：初始缩放比例，也即是当页面第一次 load的时候缩放比例。  
- maximum-scale：允许用户缩放到的最大比例。  
- minimum-scale：允许用户缩放到的最小比例。  
- user-scalable：用户是否可以手动缩放  


**共同点：量取的时候，所量尺寸，占设计稿多少rem，实际上css尺寸就占屏幕多少rem，设计稿的总宽度占多少rem，跟实际屏幕一样**  
**网易：按设计稿每100px,作为一份（1rem长度），总共rem数不确定**  
**淘宝：平均分为把宽度分为10rem**


### 网易做法



(自己理解，设计稿750，设备375)
- 设计稿（750）/100（把设计稿按每100px分为1份（1rem）总共7.5份）
- font-size= 设备（375px）/7.5=50(px),用说明当前设备每份（1rem）的长度是50px，
- 用当前的设计稿量出来的长度/100，看他占了多少份（rem),也就是他的真实rem

```
<meta name="viewport" content="initial-scale=1,
    maximum-scale=1, minimum-scale=1 ">
自己悟的<meta name="viewport" content="width=device-width,
     initial-scale=1,maximum-scale=1, minimum-scale=1 ">
```

（1）先拿设计稿竖着的横向分辨率除以100得到body元素的宽度：
如果设计稿基于iphone6，横向分辨率为750，body的width为750 / 100 = 7.5rem
    如果设计稿基于iphone4/5，横向分辨率为640，body的width为640 / 100 = 6.4rem
（播放器高度为210px，写样式的时候css应该这么写：height: 2.1rem）

```
document.documentElement.style.fontSize = document.documentElement.clientWidth / 6.4 + 'px';
//pc屏了
var deviceWidth = document.documentElement.clientWidth;
    if(deviceWidth > 640) deviceWidth = 640;
    document.documentElement.style.fontSize = deviceWidth / 6.4 + 'px';
```


另外font-size可能需要额外的媒介查询，并且font-size不能使用rem，如网易的设置：

```
@media screen and (max-width:321px){
    	.m-navlist{font-size:15px}
     }

      @media screen and (min-width:321px) and (max-width:400px){
    	.m-navlist{font-size:16px}
       }

	@media screen and (min-width:400px){
    		.m-navlist{font-size:18px}
	}
```

### 淘宝的做法：
(自己理解)
- 把设备的宽度设置成跟设计稿一样的宽度
- 1rem的宽度=把设计稿宽度/10（分成10份，10rem）
- 最后？rem= 量出来的宽度/1rem的宽度


**（这个device-width的计算公式为：
设备的物理分辨率/(devicePixelRatio * scale)，在scale为1的情况下，device-width = 设备的物理分辨率/devicePixelRatio 。
devicePixelRatio称为设备像素比，每款设备的devicePixelRatio都是已知，并且不变的，目前高清屏，普遍都是2，不过还有更高的，比如2.5, 3 等，我魅族note的手机的devicePixelRatio就是3。）**

通过js设置viewport的方法如下：

```
var scale = 1 / devicePixelRatio;
    document.querySelector('meta[name="viewport"]').setAttribute('content','initial-scale=' + scale + ', maximum-scale=' + scale + ', 			             		minimum-scale=' + scale + ', user-scalable=no');

动态计算html的font-size
document.documentElement.style.fontSize = document.documentElement.clientWidth / 10 + 'px';
```
布局的时候，各元素的css尺寸=设计稿标注尺寸/（设计稿横向分辨率/10）




### 参考资料
[淘宝&网易做法](http://www.cnblogs.com/lyzg/p/4877277.html#)  
[lib-flexible](https://github.com/amfe/lib-flexible)  
[鑫哥博客](http://www.zhangxinxu.com/wordpress/2012/08/window-devicepixelratio/)  
[其他博客new](http://www.cnblogs.com/lovesueee/p/4618454.htm)  
[其他博客](http://yunkus.com/physical-pixel-device-independent-pixels/)  
[其他博客](https://www.jianshu.com/p/bb76c606f0b4)  
[示意图](http://www.cnblogs.com/zdhblog/p/6845618.html)   
[7种方法解决移动端Retina屏幕1px边框问题](https://www.jianshu.com/p/7e63f5a32636)  

