- [盒模型](#盒模型)
  - [IE 盒模型 border-box](#ie-盒模型-border-box)
  - [W3C 标准盒模型 content-box](#w3c-标准盒模型-content-box)
- [display 常用属性值](#display-常用属性值)
- [position](#position)
- [浏览器的默认布局](#浏览器的默认布局)
- [flex 与 grid 布局](#flex-与-grid-布局)
  - [flex 布局](#flex-布局)
  - [grid 布局](#grid-布局)
- [CSS 选择器](#css-选择器)
  - [选择器命名规则](#选择器命名规则)
- [伪元素 与 伪类](#伪元素-与-伪类)
  - [伪元素](#伪元素)
  - [伪类](#伪类)
- [常见的继承属性](#常见的继承属性)
- [BFC](#bfc)
  - [创建 BFC](#创建-bfc)
  - [BFC作用](#bfc作用)
  - [IFC 行级格式化上下文](#ifc-行级格式化上下文)
- [CSS 中的各种单位](#css-中的各种单位)
- [rgba() 与 opacity 的透明效果有什么不同](#rgba-与-opacity-的透明效果有什么不同)
- [重置元素的属性值到初始值](#重置元素的属性值到初始值)
- [外边距折叠](#外边距折叠)
- [CSS 加载方式](#css-加载方式)
- [CSS 加载与页面渲染](#css-加载与页面渲染)
- [重排与重绘](#重排与重绘)
  - [重排](#重排)
  - [重绘](#重绘)
- [隐藏元素](#隐藏元素)
- [重置浏览器默认样式 Reset.css 和 Normalize.css](#重置浏览器默认样式-resetcss-和-normalizecss)
- [SaSS](#sass)
  - [Webpack 处理 SASS 文件](#webpack-处理-sass-文件)



# 盒模型

从内到外包含 content，padding，margin，border四部分

一个元素的实际宽度（高度）是 content + padding + border 的值，也就是 border-box 的值

## IE 盒模型 border-box

width，height 包含 content，padding 和 border

## W3C 标准盒模型 content-box

width，height 只包含 content，不包含 border 和 padding

# display 常用属性值

- inline 行内元素
   - 不能改变 height，width 的值，大小由内容撑开
   - padding 上下左右都有效，margin 只有左右有效

- block 块级元素
   - 能够改变 height，width 的值，默认填满父级元素的宽度
   - padding，margin 都有效

- inline-block 行内块元素
   - 能够改变 height，width 的值，默认填满父级元素的宽度
   - padding，margin 都有效
   - 如 input，img 都是 inline-block 元素

# position

- absolute 绝对定位，脱离文档流，相对于 static 以外的第一个父元素进行定位，如果不存在就逐级向上排查，直到相对于 body 元素，即浏览器窗口 

- fixed 绝对定位，脱离文档流，相对于浏览器窗口，元素的位置在屏幕滚动时不会改变

- relative 相对定位，相对于浏览器窗口

- static 默认值，元素出现在正常的流中，此时 top/right/bottom/left/z-index 无效

- sticky 依赖于用户的滚动，在 position:relative 与 position:fixed 定位之间切换（滚动最近overflow不为visible父元素，未被卷曲时表现为未创建BFC的相对定位，将被卷曲时表现为绝对定位）

- inherit 从父元素继承

# 浏览器的默认布局

默认为流动布局，包括块布局和内联布局

# flex 与 grid 布局

flex 依靠轴线布局，可以看作是一维布局，grid 是将容器划分为行和列依靠单元格布局，可以看作是二维布局

## flex 布局

- justify-content （主轴）

    flex-start | flex-end | center | space-between | space-around

- align-items （交叉轴）

    flex-start | flex-end | center | stretch | baseline（项目第一行文字的基线对齐）

- align-content （多条轴线）

    flex-start | flex-end | center | space-between | space-around | stretch

- flex-wrap 属性规定flex容器是单行或者多行

    wrap | nowrap | wrap-reverse
	
- flex 三个属性的简写，默认为 0, 1, auto

   - flex-grow 放大比例，默认为0
   - flex-shrink 缩小比例，默认为1
   - flex-basis 项目占据的主轴空间
   - auto (1, 1, auto)
   - none (0, 0, auto)
   
   flex: 1 === flex: 1 1 0%
   
- align-self 
    允许单个项目与其他项目对齐方式不同，auto为继承父元素属性，没有父元素则为 stretch

## grid 布局

- grid-template-columns(rows) 定义列宽（行高）

- grid-gap: <grid-row-gap> <grid-column-gap> 定义行（列）间距

- grid-template-areas 将多个单元格合并为一个区域

- grid-auto-flow

- justify-items 单元格内容的水平位置

- align-items 单元格内容的垂直位置

- justify-content 整个内容区域在容器里面的水平位置

- align-content 整个内容区域的垂直位置

- justify-self/align-self 单元格内容的水平位置，跟 justify-items/align-items 的用法完全一致，但只作用于单个项目

# CSS 选择器

优先级别：!important > 行内样式 > ID 选择器 > 类选择器 = 伪类 = 属性 > 标签 = 伪元素 > 通配符 > 继承 > 浏览器默认属性

## 选择器命名规则

- ID 选择器以 # 开头
- 类选择器以 . 开头

# 伪元素 与 伪类

## 伪元素

一般用来匹配特殊位置

::after | ::before

## 伪类

- -匹配特殊状态
:hover | :link | :visited | :active

- 选中元素
elem:nth-child | elem:nth-of-type | :not(elem)

- 表单状态
:enabled | :disabled | :checked

# 常见的继承属性

- 字体 font 系列
- 文本 text-align text-ident line-height letter-spacing
- 颜色 color
- 列表 list-style
- 可见性 visibility
- 光标 cursor

非继承属性：opacity，background

# BFC 

block formatting context 块级格式化上下文

BFC 相当于一个独立的容器，内部元素的布局和外部互不影响，内部元素会在垂直方向一个接一个地放置

## 创建 BFC

- 根元素 <html> 本身就是一个 BFC 区域
- position:absolute 和 position:fixed
- display: flow-root
- display: inline-block
- display: flex
- overflow: hidden

## BFC作用

- 可以解决margin上下边距重叠
- 可以解决高度塌陷
- 可以实现自适应的多栏布局

## IFC 行级格式化上下文

子元素水平方向横向排列，只会计算横向样式空间，在垂直方向上，子元素会以不同形式来对齐（vertical-align）

# CSS 中的各种单位

- px 像素
- em 相对于当前字体的大小
- rem 相对于根元素字体的大小
- vw 相对于浏览器窗口宽度的1%
- vh 相当于浏览器窗口高度的1%

# rgba() 与 opacity 的透明效果有什么不同

opacity 作用于元素，以及元素内的所有内容的透明度，rgba()只作用于元素的颜色或其背景色

设置rgba透明的元素的子元素不会继承透明效果

# 重置元素的属性值到初始值

- initial：将属性设为W3C规范初始值
- all：将元素的所有属性重置

# 外边距折叠

相邻块盒子的上下边距没有累加，而是重叠取其中最大值的现象

避免外边距重叠的方法：

- 设置边框
- 设置内边距
- 增加内容，直接写入或使用伪元素
- 触发 BFC 块级格式上下文的方法都可以

# CSS 加载方式

- style属性（内联样式）：定义的属性优先级高于所有选择器和其它加载方式

- <style></style>标签（嵌入样式）：只对当前页面有效

- <link>（外链样式）：CSS 和 HTML 分离，便于复用和维护

- @import（导入样式）：模块化，需先下载并解析引用 CSS，才可以继续下载导入 CSS，现代通常使用模块化工具在编译时合并 CSS

# CSS 加载与页面渲染

CSS 加载不阻碍 DOM 树的加载。但 CSS 会被转为 CSS 树进而与 DOM 树结合成 Render 树等待渲染，如果它加载得很慢会影响后续渲染。

通过媒体查询，可以设置link引用的 CSS 不阻塞渲染，但仍会下载。

通常情况下，CSS和不依赖 DOM 的JS放</head>标签前，依赖 DOM 的JS放在</body>标签前。

![](https://pic.leetcode-cn.com/1617075837-kYpSTD-image.png)

# 重排与重绘

渲染主要分为三个步骤：

- 布局：计算盒模型的位置，大小
- 绘制：填充盒模型的文字、颜色、图像、边框和阴影等可视效果
- 合并：所有图层绘制后，按层叠顺序合并为一个图层

重新渲染一般有三种执行路径：

- 重排：布局 → 绘制 → 合并
- 重绘：绘制 → 合并
- 合并


## 重排

引起重排的属性，即布局类属性，包括：
display padding margin width height min-height max-height border border-width 
position top bottom left right float clear
font-family font-size font-weight line-height text-align vertical-align white-space overflow overflow-y

## 重绘

引起重绘的属性，即绘制类属性，包括：

color 	visibility 	text-decoration 	box-shadow
border-color border-style border-radius
background background-image background-position background-repeat background-size 
outline outline-color outline-style outline-width

# 隐藏元素

- display: none 不占位
- visibility: hidden 占位
- opacity: 0 占位

# 重置浏览器默认样式 Reset.css 和 Normalize.css

共同点：
- 两者都能抹平浏览器间的默认样式差异
- 都部分重置了浏览器默认样式，尤其是内外边距属性

不同点：
- Reset.css让元素在不同浏览器样式完全一样
- Normalize.css适当保留部分浏览器默认样式，只重置影响开发的样式，此外
- Normalize.css修复了表单、SVG溢出等BUG
- Normalize.css适当提高了可用性
- Normalize.css避免大量使用群组选择器，通过注释提高调试体验

# SaSS

- 变量：支持$标识符变量，使用#{}插值
- 嵌套：SCSS 支持{ }大括号嵌套 SASS 支持缩进嵌套
- 扩展 / 继承 / 运算符 / @import：支持
- 流程：支持if else条件判断，支持for while each循环
- 映射：支持$()声明 Map，提供map-get(map, key) map-keys(map) map-values(map)等一系列方法操作 Map，支持遍历 Map
- 特有：支持 compass ，内含 自动私有前缀 等一系列有用SASS模块，支持压缩输出

## Webpack 处理 SASS 文件

- sass-loader
    - 将 SASS / SCSS 文件编译成 CSS
    - 调用node-sass，支持options选项向node-sass传参

- css-loader
    - 支持读取 CSS 文件，在 JS 中将 CSS 作为模块导入
    - 支持 CSS 模块 @规则@import @import url()

- style-loader
    - 将 CSS 以<style>标签的方式，插入 DOM

Webpack中 loader 的加载顺序是从后往前
