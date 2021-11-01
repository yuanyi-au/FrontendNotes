- [水平居中](#水平居中)
- [垂直居中](#垂直居中)
- [多列等高](#多列等高)
- [两栏布局](#两栏布局)
- [三栏布局](#三栏布局)
- [瀑布流布局](#瀑布流布局)
- [品布局](#品布局)
- [画一个三角形](#画一个三角形)
- [画一个九宫格](#画一个九宫格)

# 水平居中

- 行内元素，设置父元素 text-align: center
- 元素宽度固定，设置左右 margin 为 auto
- 元素宽度固定，使用绝对定位，设置元素 margin-left 为其宽度的一半；
- position:relative/position:absolute/position:fixed/position:sticky 脱离文档流后，left:0; right:0; margin:0 auto; 宽度固定
- flex布局，justify-content: center

# 垂直居中

- position:relative/position:absolute/position:fixed/position:sticky 脱离文档流后，bottom:0; top:0; margin:auto 0；或者 top:50%，margin-top 为高度一半的负值
- flex布局，align-item: center

# 多列等高

```
height: 100px;
align-items: stretch;
```

# 两栏布局

```
.left {
  flex:0 0 200px;
}
.right {
  flex: auto
}
```

# 三栏布局

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

# 瀑布流布局

```
.content {
    column-count: 3;
    column-gap: 5px;
  }
  .content > div {
    margin-bottom: 5px;
    break-inside: avoid; //避免元素在分栏时中断
    color: white;
  }
  
/** 弹性布局 **/
.content {
  display: flex;
}
.column {
  flex: 1;
  display: flex;
  flex-direction: column;
  margin-right: 5px;
}
```

# 品布局

第一行横跨两列，第二行分两栏

```
/** 弹性布局 **/
.flex {
  display: flex;
  flex-wrap: wrap;
}
.flex div:first-child {
  width: 100%;
}
.flex div:first-child ~ div {
  width: 50%;
}

/** 网格布局 **/
.grid {
  display: grid;
}
.grid div:first-child {
  grid-column-start: 1;
  grid-column-end: 3;
}
```

# 画一个三角形

当 width 和 height 都为0时，盒子尺寸就等于 border 尺寸，相当于以对角线将盒子分成四块，其他三块设为透明，剩下的一块有颜色，看起来就是一个直角三角形

```
width: 0;
height: 0;
border-color: transparent, transparent, transparent, red;
```

# 画一个九宫格

- grid

```
grid-template-columns: repeat(3, 1fr);
grid-template-rows: repeat(3, 1fr);
```

- flex

```
flex-wrap: wrap
width: 33.3%;
```

- position: relative

```
height: 33.3%;
weight:33.3%;
```