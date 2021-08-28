# 目录

# 正文

## 盒模型

从内到外包含 content，padding，margin，border四部分

一个元素的实际宽度（高度）是 content + padding + border 的值，也就是 border-box 的值

### IE 盒模型 border-box

width，height 包含 content，padding 和 border

### W3C 标准盒模型 content-box

width，height 只包含 content，不包含 border 和 padding

## display 常用属性值

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

## position

- absolute 绝对定位，脱离文档流，相对于 static 以外的第一个父元素进行定位，如果不存在就逐级向上排查，直到相对于 body 元素，即浏览器窗口 

- fixed 绝对定位，脱离文档流，相对于浏览器窗口，元素的位置在屏幕滚动时不会改变

- relative 相对定位，相对于浏览器窗口

- static 默认值，元素出现在正常的流中，此时 top/right/bottom/left/z-index 无效

- sticky 依赖于用户的滚动，在 position:relative 与 position:fixed 定位之间切换

- inherit 从父元素继承

## flex 与 grid 布局

flex 依靠轴线布局，可以看作是一维布局，grid 是将容器划分为行和列依靠单元格布局，可以看作是二维布局

### flex 布局

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

### grid 布局

- grid-template-columns(rows) 定义列宽（行高）

- grid-gap: <grid-row-gap> <grid-column-gap> 定义行（列）间距

- grid-template-areas 将多个单元格合并为一个区域

- grid-auto-flow

- justify-items 单元格内容的水平位置

- align-items 单元格内容的垂直位置

- justify-content 整个内容区域在容器里面的水平位置

- align-content 整个内容区域的垂直位置

- justify-self/align-self 单元格内容的水平位置，跟 justify-items/align-items 的用法完全一致，但只作用于单个项目

## CSS 选择器

优先级别：!important > 行内样式 > ID选择器 > 类选择器 = 伪类 = 属性 > 标签 = 伪元素 > 通配符 > 继承 > 浏览器默认属性

## 伪元素 与 伪类

### 伪元素

一般用来匹配特殊位置

::after | ::before

### 伪类

- -匹配特殊状态
:hover | :link | :visited | :active

- 选中元素
elem:nth-child | elem:nth-of-type | :not(elem)

- 表单状态
:enabled | :disabled | :checked

## BFC 规范

block formatting context 块级格式化上下文

BFC 相当于一个独立的容器，内部元素的布局和外部互不影响，内部元素会在垂直方向一个接一个地放置

### 创建 BFC

- 根元素 html 本身就是一个 BFC 区域
- 浮动定位和绝对定位
- display: inline-block
- display: flex
- overflow: hidden

### BFC作用

- 可以解决margin上下边距重叠
- 可以解决高度塌陷
- 可以实现自适应的多栏布局

### IFC 行级格式化上下文

子元素水平方向横向排列，只会计算横向样式空间，在垂直方向上，子元素会以不同形式来对齐（vertical-align）

## CSS 中的各种单位

- px 像素
- em 相对于当前字体的大小
- rem 相对于根元素字体的大小
- vw 相对于浏览器窗口宽度的1%
- vh 相当于浏览器窗口高度的1%

## rgba() 与 opacity 的透明效果有什么不同

opacity 作用于元素，以及元素内的所有内容的透明度，rgba()只作用于元素的颜色或其背景色

设置rgba透明的元素的子元素不会继承透明效果

# 各种实现方法

## 水平居中

- 行内元素，设置父元素 text-align: center
- 元素宽度固定，设置左右 margin 为 auto
- 元素宽度固定，使用绝对定位，设置元素 margin-left 为其宽度的一半；
- 元素为绝对定位，设置父元素 position 为 relative ，元素 left:0; right:0; margin:auto;
- flex布局，justify-content: center

## 垂直居中

- 绝对定位，bottom:0; top:0; margin:auto
- 绝对定位，top:50%，margin-top 为高度一半的负值
- flex布局，align-item: center

## 多列等高

- flex

```
height: 100px;
align-items: stretch;
```

## 两栏布局

### flex

```
.left {
  flex:0 0 200px;
}
.right {
  flex: auto
}
```

## 三栏布局

### flex

```
.left {
  flex: 0 0 200px;
}
.right {
  flex: 0 0 200px;
}
.center {
  flex: auto
}
```

## 画一个三角形

当 width 和 height 都为0时，盒子尺寸就等于 border 尺寸，相当于以对角线将盒子分成四块，其他三块设为透明，剩下的一块有颜色，看起来就是一个直角三角形

```
width: 0;
height: 0;
border-color: transparent, transparent, transparent, red;
```

## 画一个九宫格

### grid

```
grid-template-columns: repeat(3, 1fr);
grid-template-rows: repeat(3, 1fr);
```

### flex

```
flex-wrap: wrap
width: 33.3%;
```

### position: relative

```
height: 33.3%;
weight:33.3%;
```