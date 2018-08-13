---
layout: post
title: BFC的理解 —— 1.怎么形成bfc2.bfc的特点3.bfc的应用
categories: html、css
description: BFC的理解
keywords: html、css、bfc
---

BFC(Block Formatting Context，块格式上下文)是Web页面的可视化CSS渲染的一部分，是布局过程中生成块级盒子的区域，也是浮动元素与其他元素的交互限定区域。对其的理解能够帮你更好的布局页面。

##常见的定位方案
在进入BFC话题前，先来捋一下常见的定位方案，定位方案是控制元素的布局，主要有三种常见的方案：

###普通流(正常文档流)
在普通流中，元素按照其在HTML的先后位置至上而下布局，在这个过程中，行内元素水平排列，直到当行被占满然后换行，块级元素则被渲染为完整的一个新行，除非另外指定，否则所有元素默认都是流定位。也可以说，普通流中元素的位置由该元素在HTML文档中的位置决定。

### 浮动
在浮动布局中，元素首先按照普通流的位置出现，然后根据浮动的方向尽可能的向左边或右边偏移，其效果和印刷排版中的文本环绕相似。如下：

###绝对定位
在绝对定位布局中，元素会整体脱离普通流（浮动布局可以理解成脱离父元素文本流），因此绝对定位不会对其兄弟元素造成影响，而具体的位置由绝对定位的坐标决定。
参考的文章：http://reng99.cc/2018/08/12/about-BFC/

**目录**

* 普通文档流 与 BFC 文档流
* BFC的含义与触发条件
* BFC的特性
* BFC的应用（使用技巧）

## 什么是BFC

Formatting context（格式上下文）是W3C CSS2.1规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。

BFC即Block Formatting Content（块级格式上下文），它属于上述定位方案的普通流。具有BFC特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且BFC具有普通容器所没有的一些特性。后面介绍。

简单来说，可以把BFC理解成一个封闭的大箱子，箱子内部的元素无论如何翻江倒海，都不会影响到外部的元素。

## 触发BFC的条件

* 根元素或包含根元素的元素，这里我理解为body元素
* 浮动元素（元素的float不是none）
* 绝对定位元素（元素的position为absolute或fixed）
* 行内块元素（元素的display为inline-block）
* 表格单元格（元素的display为table-cell，html表格单元格默认为该值）
* 表格标题（元素的display为table-caption，html表格标题默认为该值）
* 匿名表格单元格元素（元素的display为table、table-row、table-row-group、table-header-group、table-footer-group（分别是html table、row、tbody、 thead、tfoot的默认属性）或inline-table）
* overflow值不为visible的块元素
* display值为flow-root的元素
* contain值为layout、content或strict的元素
* 弹性元素（display为flex或inline-flex元素的直接子元素）
* 网格元素（display为grid或inline-grip元素的直接子元素）
* 多列容器（元素的column-count或column-width不为auto，包括column-count为1）
* column-span为all的元素始终会创建一个新的BFC，即使该元素没有包裹在一个多列容器中

创建了块格式上下文的元素中的所有内容都会被包含在BFC中。以上的创建方式参考自[块格式化上下文](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)

## BFC的特性（作用）

1. 内部的box会在垂直方向，从顶部开始一个接一个地放置
2. box垂直方向的距离由margin决定。属于同一个BFC的两个相邻的box的margin会发生叠加，结果值并集
3. 在BFC中，每个盒子的左外边缘（margin-left）会触碰到容器的左边缘（border-left）。（对于从右到左的格式来说，则触碰到右边缘），即使是浮动也是如此。即不会发生margin穿透。
4. 形成了BFC的区域不会与float box重叠（可阻止因浮动元素引发的文字环绕现象）
5. 计算BFC高度时，浮动元素也参与计算（BFC会确切包含浮动的子元素，即闭合浮动）

## BFC的一些应用

* 实现自适应的两栏布局 (应用了第四点BFC的区域不会与float box重叠的特性。一边浮动，另一边自适应的部分形成BFC，那么两者就不会重叠，避免了文字环绕及类似情形。)

```html
<body>
	<div class="container">
        <div class="left">left</div>
        <div class="right">right</div>
	</div>
</body>
```
```css
html,body{
    margin: 0;
    padding: 0;
}
body{
    padding: 50px;
}
.container {
    width: 100%;
}
.left {
    width: 100px;
    height: 150px;
    background-color: blue;
    float: left;
}
.right {
    height: 200px;
    background-color: red;
    overflow: hidden; /*把.right这个自适应的box变成BFC，避免与.left box这个有float属性的元素重叠*/
}
```
* 解决父元素高度塌陷
高度塌陷产生的原因：父元素未设置固定的高度，完全由子元素高度撑开；当子元素float之后脱离了文档流，父元素内部就没有支撑，造成了高度的塌陷。

也就是用到了BFC的特性五：闭合内部浮动。

```html
<body>
	<div class="container">
        <div class="left">left</div>
        <div class="right">right</div>
	</div>
</body>
```
```css
html,body{
    margin: 0;
    padding: 0;
}
body{
    padding: 50px;
}
.container {
    width: 100%;
}
.left {
    width: 100px;
    height: 150px;
    background-color: blue;
    float: left;
}
.right {
    height: 200px;
    background-color: red;
    overflow: hidden; /*把.right这个自适应的box变成BFC，避免与.left box这个有float属性的元素重叠*/
}
```
* margin重叠解决

```html
<body>
    <div class="top">top</div>
    <div class="bottom">bottom</div>
</body>
```
```css
.top {
    width: 100px;
    height: 100px;
    background-color: blue;
    margin-bottom: 100px;
}
.bottom{
    width: 100px;
    height: 100px;
    background-color: red;
    margin-top: 50px;
}
```
上面出现的情况是BFC的特性三：属于同一个BFC的两个相邻的box的margin会发生叠加，结果值并集。那么，我们将他们隔离成不同的BFC不久解决问题了嘛。
```html
<body>
    <div class='wrap'>
        <div class="top">top</div>
    </div>
    <div class="bottom">bottom</div>
</body>
```
```css
.top {
    width: 100px;
    height: 100px;
    background-color: blue;
    margin-bottom: 100px;
}
.bottom{
    width: 100px;
    height: 100px;
    background-color: red;
    margin-top: 50px;
}
.wrap{
    overflow: hidden; /*将.top box包含在另外一个BFC中隔离开*/
}
```
