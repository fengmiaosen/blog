---
title: CSS实现元素宽高比自适应
date: 2016-08-31 09:57:30
tags: [CSS]
categories: 
    - CSS
---
> 前端开发需求经常需要适配不同尺寸的屏幕，有时需要保证某些元素在不同屏幕下仍然能保持宽高比自适应，即所谓的响应式开发要求。
> 而通过纯CSS来实现的核心技术要点是：当margin/padding取形式为百分比的值时，无论是left/right，还是top/bottom，都是以**父元素的width**为参照物。这里列举了常用的三种方法来实现宽高自适应。

<!-- more -->

# 方法一： 设置元素的*padding-top* 或者 *padding-bottom* 

由于元素竖直方向的padding-top和padding-bottom属性使用百分比来赋值时，是相对元素最近的**父容器**宽度 width来计算，所以我们可以利用这个特性来保证不同屏幕条件下的元素等宽高比。
例如：一个div元素的宽度是 100px，设置其padding-top 为75%，则计算所得为75px，宽高比为4:3；如果width为20%，padding-top为75%，计算后padding-top为20% * 75%，最终宽高比仍然为 4:3。
## 使用方法
```js
div {
    height: 0;
    width: 100%;
    padding-bottom: 75%; /* 4:3 */
}
```
## 盒模型示意
![box](http://7xt6po.com1.z0.glb.clouddn.com/blog/box.png)

## 示例demo

{% jsfiddle fengmiaosen/bypsnmjy  html,css,result dark  %}


# 方法二： 使用伪类 *::before*  或 *::after* 设置padding属性
## 使用方法
```js
.element {
    width: 20%; 
    position: relative;
}

.element:before {
    display: block;
    content: "";
    padding-top: 75%; /* 4:3 */
}
```
##  示例demo

{% jsfiddle fengmiaosen/kpybfn6a  html,css,result  dark  %}

# 方法三： 使用视区相关单位vm、vh单位设置元素宽高

##  单位含义

> 视窗viewport — 为浏览器内部的可视区域大小，即window.innerWidth/window.innerHeight大小，不包含任务栏标题栏以及底部工具栏的浏览器区域大小
> vm —  相对于视窗的宽度：视窗宽度是100vm，假设视窗的像素宽度是 500px，则1vw == 5px
> vh — 相对于视窗的宽度：视窗宽度是100vh

## 浏览器兼容性支持
[点击查看](http://caniuse.com/#feat=viewport-units)
![vmvh](http://7xt6po.com1.z0.glb.clouddn.com/blog/vmvh.png)
## 使用方法
```js
.element {
    width: 20vw;
    height: 15vw;
}
```
## 示例demo

{% jsfiddle fengmiaosen/1kgczxnp   html,css,result  dark  %}

# 参考资料
* [Maintaining Ratio Of Elements: All The Methods](http://xtianmiller.com/notes/maintaining-ratio-of-elements-all-the-methods/)
* [巧用margin/padding的百分比值实现高度自适应](https://segmentfault.com/a/1190000004231995)
* [视区相关单位vw, vh..简介以及可实际应用场景](http://www.zhangxinxu.com/wordpress/2012/09/new-viewport-relative-units-vw-vh-vm-vmin/)


