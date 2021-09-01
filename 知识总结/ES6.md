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
Function.prototype.bind = function (...arg1) {
  //保存this，这里的this指的是test方法
  let self = this;
  //取出第一个参数，也就是this的指向对象
  let obj = arg1[0];
  if (typeof obj === "object") {
    obj = obj || window; //  obj 如果是null就会赋值为window
  } else {
    obj = Object.create(null);
  }
  //剩余参数保存起来
  let params1 = arg1.slice(1);
  //返回一个待执行的函数
  return function (...arg2) {
    //和第一个参数合并为完整的参数
    let params2 = params1.concat(arg2);
    //调用apply方法进行this转换
    return self.apply(obj, params2);
  };
};
var test = function (a, b, c) {
  console.log("x=" + this.x, "a=" + a, "b=" + b, "c=" + c);
};
var o = {
  x: 1,
};
test.bind(o, 1)(2, 3); //x=1 a=1 b=2 c=3
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

### 链式调用

连续执行多个异步操作，在上一个操作执行成功后开始执行下一个操作
Promise 链 then() 函数

### ES6的class与ES5的继承的区别

ES5继承：

基本思想：利用原型链让一个引用类型继承另一个引用类型的属性和方法（即通过prototype和构造函数实现）
实质：将父类添加到子类的原型链上去

ES6继承：

基本思想：通过extend关键字实现继承，子类可以继承父类中所有的方法和属性，子类必须在construct()方法中调用super()方法，因为新建的子类没有自己的this对象，而是继承了父类的this对象；

实质：利用extend关键字继承父类，然后继承父类的属性和方法

### 继承

原型链 | 借用构造函数 | 组合继承 | 寄生式继承 | 寄生组合式继承

### Array.from()

从一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例。