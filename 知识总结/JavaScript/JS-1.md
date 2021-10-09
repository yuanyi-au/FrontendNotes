- [JS-1](#js-1)
  - [数据类型](#数据类型)
    - [基础数据类型](#基础数据类型)
      - [Symbol](#symbol)
      - [bigInt](#bigint)
      - [null 和 undefined](#null-和-undefined)
    - [引用数据类型](#引用数据类型)
  - [Arguments 与 Array](#arguments-与-array)
    - [相同点](#相同点)
    - [不同点](#不同点)
  - [类数组](#类数组)
    - [类数组对象转数组](#类数组对象转数组)
  - [|| 与 &&](#-与-)
  - [栈(stack)与堆(heap)的区别](#栈stack与堆heap的区别)
  - [原型与原型链](#原型与原型链)
    - [获取原型的方法](#获取原型的方法)
    - [继承](#继承)
  - [闭包 （closure)](#闭包-closure)
  - [函数式编程](#函数式编程)
  - [函数柯里化](#函数柯里化)
  - [事件](#事件)
    - [事件循环机制](#事件循环机制)
  - [事件捕获 与 事件冒泡](#事件捕获-与-事件冒泡)
    - [事件委托](#事件委托)
  - [内存泄露 memory leak](#内存泄露-memory-leak)
    - [解决方法](#解决方法)
  - [函数去抖与函数节流](#函数去抖与函数节流)
    - [去抖 debounce](#去抖-debounce)
    - [节流 throttle](#节流-throttle)
  - [垃圾回收机制](#垃圾回收机制)
    - [引用计数](#引用计数)
    - [标记清除](#标记清除)
  - [内存溢出](#内存溢出)
  - [图片懒加载](#图片懒加载)
  - [异步操作](#异步操作)
    - [实现异步请求的方式](#实现异步请求的方式)
    - [回调函数 Ajax](#回调函数-ajax)
      - [XMLHttpRequest API （ Ajax 的核心）](#xmlhttprequest-api--ajax-的核心)
      - [fetch API](#fetch-api)
      - [Server-sent 事件](#server-sent-事件)
    - [事件监听 addEventListener](#事件监听-addeventlistener)
    - [观察者模式](#观察者模式)
    - [Promise（具体见 ES6 部分）](#promise具体见-es6-部分)
    - [async/await](#asyncawait)
    - [co 库的 generator 函数](#co-库的-generator-函数)
  - [跨域解决方案](#跨域解决方案)
    - [jsonp：JSON with Padding](#jsonpjson-with-padding)
    - [CORS：Cross-origin resource sharing 跨域资源共享](#corscross-origin-resource-sharing-跨域资源共享)
    - [postMessage](#postmessage)
    - [Websocket](#websocket)
    - [代理服务器转发](#代理服务器转发)
    - [document.domain](#documentdomain)
    - [iframe](#iframe)
  - [性能优化](#性能优化)
    - [衡量性能的时间点](#衡量性能的时间点)
    - [测量性能的工具](#测量性能的工具)
    - [性能优化方法](#性能优化方法)

# JS-1

## 数据类型

### 基础数据类型

String Number Boolean Undefined Null Symbol bigInt

基础数据类型没有办法添加属性与方法

存放在栈(stack)中，占据空间小，大小固定，因为需要频繁使用故放入栈中

#### Symbol

- ES6 新增，用来储存独一无二不可变的数据
- 不能与其他类型的值进行运算，会报错
- 可以显式转换为字符串，隐式转换报错；可以显式/隐式转换为布尔值，结果为true；不能显式/隐式转换为数字，报错

#### bigInt

- ES10 新增，可以表示任意大的整数，之前JavaScript 中 Number 可以表示的范围是 -2^53-1 到 2^53-1
- 不能用于 Math 对象中的方法，不能和 Number 混合运算

#### null 和 undefined

- undefined 是未定义，null 是空对象
- 用 typeof 进行判断时，undefined 返回 undefined， null 返回 object (undefined==null true, undefined===null false)

### 引用数据类型
Object Array Function Date Math RegExp

存放在堆(heap)中，占据空间大，大小不固定，如果存储在栈中会影响栈的性能

## Arguments 与 Array

### 相同点

- 都可以用下标索引访问元素
- 都有 length 属性

### 不同点

- 数组可用 `for...in` 和 `for` 循环遍历
- 类数组只可以用 `for` 遍历（因为类型是 Object）

## 类数组

定义：拥有 length 属性，若干索引属性的任意对象

如NodeList、HTML Collections、字符串、arguments

### 类数组对象转数组

- 新建数组，遍历类数组对象，将每个元素放入新数组
- Array.prototype.slice.call(ArrayLike) 或 [].slice.call(ArrayLike)
- Array.from(ArrayLike)
- apply 第二参数传入，调用数组方法

`Array.prototype.push.apply(null, ArrayLike)`


## || 与 &&

||操作符：如果条件判断结果为 true 就返回第一个操作数的值，如果为 false 就返回第二个操作数的值。&& 与之相反。

## 栈(stack)与堆(heap)的区别

- 栈是先进后出，堆是一个优先队列，完全二叉树是堆的一种实现方式
- 存放在栈中的数据保存的是值，存放在堆中的数据保存的是指针，指向堆中的内存地址
- 栈的内存由编译器自动释放，堆的内存需要程序员手动释放。如果没有手动释放，程序结束的时候可能由垃圾回收机制回收
- 栈的效率比堆高，计算机底层有专门给栈的寄存器，有专门的机器指令执行栈的操作
- 栈的空间有限制，如果请求变量过大或递归调用过深会有栈溢出(Stack Overflow)的问题，程序会被系统强制停止运行

## 原型与原型链

在 JS 里我们用构造函数创建对象，每一个对象创建后就会自动关联一个对象，这个对象就是我们所说的原型，当我们访问所创建的对象时，如果它内部没有我们需要的属性，就会到它的原型对象里寻找这个属性，像这样一直上溯，就是我们所说的原型链，可以利用`_proto_`上溯原型链。ES6 之前 JS 就是通过原型链实现“继承”的

![冯羽老师的原型链示意图](https://github.com/mqyqingfeng/Blog/raw/master/Images/prototype5.png)

蓝色的线就是原型链

### 获取原型的方法

- fn._proto_
- fn.constructor.prototype
- Object.getPrototypeOf(fn)

### 继承

原型链继承 | 构造函数继承 | 组合继承 | 寄生式继承 | 寄生组合式继承

## 闭包 （closure)

闭包就是一个函数与其周围环境捆绑在一起形成的组合，让我们可以从一个内层函数访问到外层函数作用域

在 JavaScript 中每创建一个函数的同时就会创建一个闭包

注意：闭包在处理速度和内存方面对脚本性能有负面影响，非必要时最好不用

## 函数式编程

函数式编程是声明式编程的一部分

- 不可变性：无法更改数据，只能通过复制 (Object.assign) 副本来更改
- 函数与其他数据类型一样可以赋值给变量，可以作为参数传入
- 只用表达式不用语句

[函数式编程初探](https://www.ruanyifeng.com/blog/2012/04/functional_programming.html)

## 函数柯里化

只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。

用途：

1.延迟计算；
2.参数复用；
3.动态生成函数

```
function add(num1, num2){
    return num1 + num2;
}

var curriedAdd = curry(add, 5);
alert(curriedAdd(3))  //8
```

## 事件

事件的流向：事件捕获 - 事件源 - 事件冒泡

### 事件循环机制

程序开始执行之后，主程序开始执行同步任务（宏任务），碰到异步任务就把它放到任务队列中,等到同步任务全部执行完后，js引擎就去查看任务队列有没有可以执行的异步任务（微任务），将异步任务转为同步任务，开始执行，执行完同步任务之后继续查看任务队列，如此循环。

常见宏任务：setTimeout、setInterval、setImmediate、I/O、script、UI rendering

常见微任务：Promise、MutationObserver


## 事件捕获 与 事件冒泡

事件冒泡，就是事件开始时由最具体的元素接收，然后逐级向上传播到较为不具体的节点，最后到window对象。事件捕获与之相反

- 在支持addEventListener()的浏览器中，可以调用事件对象的 stopPropagation() 方法以阻止事件的继续传播
- e.preventDefault() 可以阻止事件的默认行为发生

### 事件委托

利用事件冒泡，只指定一个事件处理程序管理某一类型的所有事件

## 内存泄露 memory leak

内存泄漏：不再使用的内存没有及时释放或者不能释放

JS 的垃圾回收机制有引用计数的方法，如果存在有两个对象循环引用的情况，就会发生内存泄漏

如果连续五次垃圾回收后，内存占用一次比一次大，就存在内存占用。如果内存占用基本平稳，就说明不存在内存占用

[JS内存泄漏排查方法](https://cloud.tencent.com/developer/article/1444558)

### 解决方法

可以从一开始就声明需要手动清除的引用，ES6 中引入 WeakSet 和 WeakMap，它们对值的引用不计入垃圾回收机制

[JavaScript 内存泄漏教程](http://www.ruanyifeng.com/blog/2017/04/memory-leak.html)



## 函数去抖与函数节流

### 去抖 debounce

调用函数n秒后才会执行该函数，如果在n秒内调用该函数则将取消前一次并重新计算执行时间

```
function debounce(fn, delay) {
	let timer = null
	return function (...args) {
		if (timer) clearTimeout(timer)
		timer = setTimeout(() => {
			timer = null
			fn.apply(this, args)
		}, (delay + '') | 0 || 1000 / 60)
	}
}
```

### 节流 throttle

预设一个执行周期，如果调用函数的时刻大于等于执行周期，则执行该动作，然后进入下一个新的周期

```
function throttle(fn, interval) {
	let timer = null
	return function (...args) {
		if (timer) return
		timer = setTimeout(() => {
			timer = null
			fn.apply(this, args)
		}, (interval +'')| 0 || 1000 / 60)
	}
}
```

## 垃圾回收机制

### 引用计数

清除没有任何引用指向的数据

### 标记清除

删除没有被标记过，即不可达对象，可处理循环引用

缺点：内存不连续，回收效率低，偶尔卡顿

## 内存溢出

- 意外的全局变量：尽量不使用全局变量

- 未清除的计时器：使用 requestAnimationFrame / setTimeout +递归 代替 setInterval，并设置边界

- 删除不完全的对象：移除对象前，移除监听，移除 DOM 后，设置引用该 DOM 的变量为空

- 闭包中未使用函数引用了可从根访问的父级变量：`fromGlobal = null`


## 图片懒加载

懒加载也叫延迟加载，指的是在长网页中延迟加载图片的时机，当用户需要访问时，再去加载

实现原理是，将页面上的图片的 src 属性设置为空字符串，将图片的真实路径保存在一个自定义属性中，当页面滚动的时候，进行判断，如果图片进入页面可视区域内，则从自定义属性中取出真实路径赋值给图片的 src 属性，以此来实现图片的延迟加载。



## 异步操作

### 实现异步请求的方式

- AJAX
```
const xhr = new XMLHttpRequest()
xhr.open('GET', 'https://leetcode-cn.com', true)
xhr.responeseType = 'json'
xhr.onreadystatechange = function() {
  if (xhr.readyState === 4 && xhr.status === 200) {
    console.log(xhr.response)
  }
}
xhr.ontimeout = function() {
  console.log('超时')
}
xhr.setRequestHeader('Content-Type', 'application/json')
xhr.send({ requestBody: 1 })
```

- Axios
```
axios({
  url: 'https://leetcode-cn.com',
  data: {
    requestBody: 1
  }
}).then(data => console.log(data))
```

- Fetch
```
fetch('https://leetcode-cn.com', {
  requestBody: 1
}).then(res => res.json()).then(res => res)
```

### 回调函数 Ajax

Asynchronous JavaScript + XML（异步JavaScript和XML）

原理：利用 XMLHttpRequest 向服务器发送异步请求，并将服务器返回的数据交还给客户端处理，利用 JS 的 DOM 操作去更新页面，在form表单提交的时候，可实现页面不刷新，缺点是没有history(-1)

JSON 和 XML 都用于在 Ajax 模型中打包信息，由于 JSON 更加轻量而且是 JavaScript 的一部分，目前 JSON 的使用比 XML 更普遍

#### XMLHttpRequest API （ Ajax 的核心）

#### fetch API 

#### Server-sent 事件

### 事件监听 addEventListener

### 观察者模式 

### Promise（具体见 ES6 部分）

### async/await

- try catch
await 函数外，包裹 try {}，当 catch (error) 时，捕获错误

- catch
async 函数返回的 Promise，调用 then 方法，然后 catch(error)


### co 库的 generator 函数 


## 跨域解决方案

### jsonp：JSON with Padding

- <script> 标签没有同域限制
- 服务端返回页面上 callback 包裹的 json 数据

### CORS：Cross-origin resource sharing 跨域资源共享

### postMessage

跨窗口，跨框架frame、iframe，跨域

### Websocket

### 代理服务器转发

### document.domain

- 主域名相同，子域名不同
- 设置值为 主域名

### iframe

- window.name
- location.hash 加载跨域页，参数跟在 # 后



## 性能优化

### 衡量性能的时间点

- 首字节时间：收到服务器返回第一个字节的时间。反映网络后端的整体响应耗时
- 白屏时间：第一个元素显示时间
- 首屏时间：第一屏所有资源完整显示时间。SEO 指标，百度建议2秒内为宜，不超过3秒

### 测量性能的工具

- Performance：通过录制页面加载过程，可以获得加载，脚本执行，渲染，布局等时间分布情况

- Lighthouse：可以分别模拟移动端和电脑端打开 URL，从性能、可访问性、最佳实践（安全、用户体验、函数是否符合标准等）、SEO 、单页应用等角度，获得修改建议


### 性能优化方法

- 预加载
  - prefetch：<link rel="prefetch" href=" " />
  - Image / Audio

- 懒加载
  - 图片懒加载
  - DOM懒加载

- 渲染
  - 减少重排、重绘
  - 节流、防抖

- 缓存
  - 离线缓存
  - HTTP 缓存

-图片
  - 合并图片请求
    - Base64 将图片嵌入 CSS
    - SVG sprites
  - 响应式图片：不同分辨率和 DPR 下显示不同图片
    - 媒体查询
    - IMG 的 srcset 属性
  - 图片压缩
    - image-webpack-loader、imagemin-webpack-plugin 工程化压缩图片工具
    - WEBP 和 AVIF 图片格式

- HTTP
  - 使用 HTTP2 / HTTP3
  - 使用 CDN
  - 使用 gzip / br 压缩静态资源
