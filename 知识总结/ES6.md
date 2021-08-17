# 目录

# 正文

## var, let, const 的区别

- var 没有块级作用域，支持变量提升
- let 有块级作用域，不支持变量提升，不允许重复声明，有暂时性死区
- const 有块级作用域，不支持变量提升，不允许重复声明，有暂时性死区
- const 声明的变量的内存地址不能变动，改变会报错，值可以改变

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

## Promise

Promise 是一个对象，代表异步操作的最终完成或者失败，可以在上面绑定回调函数
Promise有三种状态：pending（进行中），fulfilled（已成功），rejected（已失败）
- 只有异步操作的结果能决定 Promise 的状态，状态第一次改变后就凝固不变了

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

### 链式调用

连续执行多个异步操作，在上一个操作执行成功后开始执行下一个操作
Promise 链 then() 函数



