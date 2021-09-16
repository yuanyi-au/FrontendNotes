# 目录

# 正文

## var, let, const 的区别

- var 没有块级作用域，支持变量提升
- let 有块级作用域，不支持变量提升，不允许重复声明，有暂时性死区
- const 有块级作用域，不支持变量提升，不允许重复声明，有暂时性死区
- const 声明的变量的内存地址不能变动，改变会报错，值可以改变

- let 的「创建」过程被提升了，但是初始化没有提升
- var 的「创建」和「初始化」都被提升了
- function 的「创建」「初始化」和「赋值」都被提升了

### TDZ 暂时性死区

const 与 let 在声明前使用会报错，它只能在绑定的块级作用域内使用

### 块级作用域

ES6 之前没有块级作用域，内层变量可能会覆盖外层变量，用来计数的循环变量可能会泄露成为全局变量

## 模板字符串

允许内嵌表达式，使用反引号 (``) ，可以包含占位符 (${expression})

## 箭头函数

- 箭头函数没有 this，会向外层查找 this 的值
- 不能用 call() apply() bind() 方法改变 this 的指向
- 没有 arguments 对象
- 没有 construct 方法，不能 new，也没有原型

## call，apply，bind

call()、apply()、bind()是用来改变this的指向的，第一个参数都是 this 的指向对象

call() 和 apply() 传参不同，后者第二个参数是数组

bind() 返回的是函数

### 相同点

都是用来改变 this 的指向

### 不同点

- call 和 apply 改变了 this 指向后返回执行结果，而 bind 是返回新的函数，需要调用
- call 与 apply 主要是传参不同
   `fn.call(obj, arg1, arg2...);`
   `fn.apply(obj,[arg1, arg2...]);`

### 手写 bind()

```
Function.prototype.myBind = function(_this, ...args) {
	const fn = this
	return function F(...args2) {
		return this instanceof F ? new fn(...args, ...args2)
		: fn.apply(_this, args.concat(args2))
	}
}
//使用
function Sum (a, b) {
	this.v= (this.v || 0)+ a + b
	returnthis
}
const NewSum = Sum.myBind({v: 1}, 2)
NewSum(3) // 调用：{v: 6}
new NewSum(3) // 构造函数：{v: 5} 忽略 myBind 绑定this
```

### 手写 call

```
Function.prototype.myCall = function(_this, ...args) {
	if (!_this) _this = Object.create(null)
	_this.fn = this
	const res = _this.fn(...args)
	delete _this.fn
	return res
}
// 使用
function sum (a, b) {
	return this.v + a + b
}
sum.myCall({v: 1}, 2, 3) // 6
```


### 手写 apply

```
Function.prototype.myApply = function(_this, args = []) {
	if (!_this) _this = Object.create(null)
	_this.fn =this
	const res = _this.fn(...args)
	delete _this.fn
	return res
}
// 使用
function sum (a, b) {
	return this.v + a + b
}
sum.myApply({v: 1}, [2, 3]) // 6
```


## Promise

Promise 是一个对象，代表异步操作的最终完成或者失败，可以在上面绑定回调函数
Promise有三种状态：pending（进行中），fulfilled（已成功），rejected（已失败）

只有异步操作的结果能决定 Promise 的状态，状态第一次改变后就凝固不变了

### Promise 的缺点：

- 无法取消，新建后立即执行
- 如果没有回调函数， Promise 内部抛出的错误无法反应到外部

### Promise.all()

将多个 Promise 实例包装成一个新的 Promise 实例

新的实例的状态由之前的所有实例所决定，之前的全都 fulfilled，新实例的状态才会是 fulfilled，否则只要有一个被 rejected，新实例的状态就是 rejected，第一个被 rejected 的实例的返回值会传递给新实例的回调函数

### Promise.race()

同样是将多个 Promise 实例包装成一个新的 Promise 实例

只要多个实例中有一个实例先改变状态，新实例的状态就跟着改变，第一个改变的实例返回值传递给新实例的回调函数

### Promise.resolve()

将现有对象转为 Promise 对象

### Promise.reject()

也是将现有对象转为 Promise 对象，但状态为 rejected

### Promise.try()

`Promise.resolve().then()`

不想区分是同步操作还是异步操作的时候，用 then 指定下一步流程，用 catch 处理抛出的错误

### 如何终止Promise

- Promises / A+ 标准
  - 原 Promise 对象的状态与新对象保持一致
  - 返回一个pending状态的 Promise 对象，后续的函数都不会调用

- Promise.race 竞速
  - 其中一个 Promise 先到达resolve状态，其他 Promise 不会执行

- 抛出错误
  - 其中一个 Promise 抛出一个错误后，错误信息将沿着 链路 向后传递，直至被捕获
  - 错误被捕获前的函数不会被调用
  
### 手写 Promise

```
const PENDING = 'pending'
const RESOLVED = 'resolved'
const REJECTED = 'rejected'
function Promise(callback) {
  this.state = PENDING
  this.value = null
  this.resolvedCallbacks = []
  this.rejectedCallbacks = []
  
  callback(value => {
    if (this.state === PENDING) {
      this.state = RESOLVED
      this.value = value
      this.resolvedCallbacks.map(callback => callback(value))
    }
  }, value => {
    if (this.state === PENDING) {
      this.state = REJECTED
      this.value = value
      this.rejectedCallbacks.map(callback => callback(value))
    }
  })
}
Promise.prototype.then = function (onFulfilled = () => {}, onRejected = () => {}) {
  if (this.state === PENDING) {
    this.resolvedCallbacks.push(onFulfilled)
    this.rejectedCallbacks.push(onRejected)
  }
  if (this.state === RESOLVED) {
    onFulfilled(this.value)
  }
  if (this.state === REJECTED) {
    onRejected(this.value)
  }
}
```

## 链式调用

连续执行多个异步操作，在上一个操作执行成功后开始执行下一个操作
Promise 链 then() 函数

## ES6的class与ES5的继承的区别

ES5继承：

基本思想：利用原型链让一个引用类型继承另一个引用类型的属性和方法（即通过prototype和构造函数实现）
实质：将父类添加到子类的原型链上去

ES6继承：

基本思想：通过extend关键字实现继承，子类可以继承父类中所有的方法和属性，子类必须在construct()方法中调用super()方法，因为新建的子类没有自己的this对象，而是继承了父类的this对象；

实质：利用extend关键字继承父类，然后继承父类的属性和方法

### Array.from()

从一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例。


### 对比 import、import() 和 requrie

-|import|import()|require
-|-|-|-
规范|ES6Module|ES6Module|CommonJS
执行阶段|静态 编译阶段|动态 执行阶段|动态 执行阶段
顺序|置顶最先|异步|同步
缓存|√|√|√|
默认导出|default|default|直接赋值
导入赋值|解构赋值，传递引用|在then方法中解构赋值，属性值是仅可读，不可修改|	基础类型 赋值，引用类型：浅拷贝



### ES6、ES7、ES8、ES9、ES10 新特性

#### ES6

let 和 const
Promise
Class
箭头函数
函数参数默认值
模版字符串
解构赋值
展开语法
对象属性缩写
模块化

#### ES7

includes()
指数操作符**

#### ES8

async/await
Object.values()
Object.entries()
Object.getOwnPropertyDescriptors()
填充字符串到指定长度：padStart、padEnd

#### ES9

异步迭代：for await (let i of array)
Promise.finally()

#### ES10

flat 和 flatMap
trimStart 和 trimEnd 去除字符串首尾空白字符
Object.fromEntries()传入键值对列表，返回键值对对象
Symbol.prototype.description
String.prototype.matchAll 返回包含所有匹配正则表达式和分组捕获结果的迭代器
Function.prototype.toString() 返回精确字符，包括空格和注释
新基本数据类型 BigInt
globalThis
import()

#### ES11+

String.prototype.replaceAll
Promise.any
WeakRefs （WeakMap、WeakSet）