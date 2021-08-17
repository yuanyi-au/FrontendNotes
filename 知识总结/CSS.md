# 目录

# 正文

## 盒模型

包含 content，padding，margin，border四部分

### IE 盒模型 border-box

width，height 包含 content，border 和 padding

### W3C 标准盒模型 content-box

width，height 只包含 content，不包含 border 和 padding

在ie8+浏览器中默认为 content-box，如果将box-sizing设为border-box则用的是IE盒模型。在ie6，7，8中DOCTYPE缺失会将盒子模型解释为IE盒子模型，若在页面中声明了DOCTYPE类型，所有的浏览器都会把盒模型解释为W3C盒模型。

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
- inherit 从父元素继承

## flex 与 grid 布局

### flex 布局

- justify-content （主轴）
flex-start | flex-end | center | space-between | space-around
- align-items （交叉轴）
flex-start | flex-end | center | stretch | baseline（项目第一行文字的基线对齐）
- align-content （多条轴线）
flex-start | flex-end | center | space-between | space-around | stretch
- flex-wrap
wrap | nowrap | wrap-reverse
- flex 三个属性的简写，默认为 0, 1, auto
   - auto (1, 1, auto)
   - none (0, 0, auto)
   - flex-grow 放大比例，默认为0
   - flex-shrink 缩小比例，默认为1
   - flex-basis 项目占据的主轴空间
- align-self 允许单个项目与其他项目对齐方式不同，auto为继承父元素属性，没有父元素则为 strech

### grid 布局

## margin 与 padding

- margin：需要在 border 外侧添加空白时，隔开元素与元素之间的间距
- padding：需要在 border 内测添加空白时，隔开元素与内容之间的间距
## CSS 选择器

- 优先级别：!important > 行内样式 > ID选择器 > 类选择器 = 伪类 = 属性 > 标签 = 伪元素 > 通配符 > 继承 > 浏览器默认属性

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

BFC 相当于形成了一个独立的区域，它内部元素的布局和外部互不影响

### 创建 BFC

- 根元素就是一个 BFC 区域
- 浮动定位和绝对定位
- display: inline-block
- display: flex
- overflow: hidden

### IFC 行级格式化上下文

## CSS 中的各种单位

- px 像素
- em 相对于当前字体的大小
- rem 相对于根元素字体的大小
- vw 相对于浏览器窗口宽度的1%
- vh 相当于浏览器窗口高度的1%

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

```
width: 0;
height: 0;
border-color: transparent, transparent, transparent, red
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

height: 33.3%;
weight:33.3%;
