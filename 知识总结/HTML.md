- [HTML](#html)
  - [<iframe> 标签](#iframe-标签)
    - [优缺点](#优缺点)
      - [优点](#优点)
      - [缺点](#缺点)
    - [<iframe> 跨域](#iframe-跨域)
  - [src 和 href 的区别](#src-和-href-的区别)
  - [link 和 @import 的区别](#link-和-import-的区别)

# HTML

## <iframe> 标签

iframe 元素会创建包含另外一个文档的内联框架（即行内框架）

### 优缺点

#### 优点

- 能够原封不动地把嵌入的网页展示出来
- 提高页面代码的复用性
- 解决加载缓慢的第三方内容，如图标和广告等的加载问题
- 在处理上传或局部刷新时，避免了页面整体刷新
- 解决部分跨域问题

#### 缺点

- 会阻塞主页面的 onload 事件
- 无法被一些搜索引擎索引到
- 页面会增加服务器的http请求
- 会产生很多页面，不便于管理
- 很多移动设备无法完全显示框架，设备兼容性差
- 会出现区域的上下、左右滚动条，滚动条会挤占页面空间
- 使用框架时，要保证正确的使用导航链接，容易造成链接死循环

### <iframe> 跨域

1、location.hash
2、window.name
3、postMessage
4、document.domain


## src 和 href 的区别

- src 用于替换当前元素，href 用于在当前文档和引用资源之间确立联系
- src 是 source的缩写，指向外部资源的位置，指向的内容将会嵌入到文档中当前标签所在的位置
- href 是 Hypertext Reference 的缩写，指向网络资源所在的位置，建立和当前元素或当前文档之间的链接

## link 和 @import 的区别

- link属于HTML标签，而 @import 是用来加载CSS的
- 页面被加载时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载
- @import只能在IE5以上才能识别，而link是HTML标签，无兼容问题
- link方式的样式权重高于@import的权重
- 当使用js控制DOM去改变样式的时候，只能使用link方式，因为 @import 只有CSS才能识别，DOM无法控制
