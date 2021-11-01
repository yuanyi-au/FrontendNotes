- [基础数组方法](#基础数组方法)
	- [元素的删除、添加、修改](#元素的删除添加修改)
	- [数组的合并、拆分](#数组的合并拆分)
- [Arguments 与 Array](#arguments-与-array)
- [判断数组的方法](#判断数组的方法)
- [类数组](#类数组)
	- [类数组对象转数组](#类数组对象转数组)
	- [Array.from](#arrayfrom)
- [Array、Set 与 Map 的遍历](#arrayset-与-map-的遍历)
- [数组拷贝](#数组拷贝)
	- [浅拷贝与深拷贝的区别](#浅拷贝与深拷贝的区别)
	- [浅拷贝方法](#浅拷贝方法)
	- [深拷贝方法](#深拷贝方法)
		- [递归调用实现深拷贝](#递归调用实现深拷贝)
		- [用 assign 方法深拷贝](#用-assign-方法深拷贝)
- [数组去重](#数组去重)
- [数组分片操作](#数组分片操作)
	- [slice()](#slice)
	- [substr() 与 substring()](#substr-与-substring)
	- [slice()、substr() 与 substring() 的区别](#slicesubstr-与-substring-的区别)
	- [split() 与 join()](#split-与-join)
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
- [正则表达式](#正则表达式)
	- [实现 trim 去除首尾空格](#实现-trim-去除首尾空格)
- [求dom元素下所有dom元素个数](#求dom元素下所有dom元素个数)
- [0.1+0.2 !=== 0.3](#0102--03)


# 基础数组方法

## 元素的删除、添加、修改

- push()：向数组末尾添加元素，返回新数组的长度

- pop()：删除数组最后一个元素，返回被删除的值

- unshift()：向数组开头添加元素，返回新数组的长度

- shift()：删除数组第一个元素，返回被删除的值

- delete 运算符：由于数组属于对象，可以使用 delete 运算符，但删除的位置会留下 undefined 空洞
	`delete arr[0]`
	
- splice()：参1是操作位置，参2是删除元素的数量，参3是要添加的新元素，返回被修改的数组

## 数组的合并、拆分

- toString()：把数组转换为数组值（逗号分隔）的字符串

- join()：将所有数组元素结合为一个字符串，类似 toString()，可以指定分隔符

- concat()：合并现有数组成为一个新数组
	`arr1.concat(arr2, arr3)`

- slice：数组切片出一个新数组，参1是开始位置，参2是结束位置

**注意：可以改变原数组的方法有：pop() push() shift() unshift() splice() sort() reverse() forEach()**

# Arguments 与 Array

- 相同点
  - 都可以用下标索引访问元素
  - 都有 length 属性

- 不同点
  - 数组可用 `for...in` 和 `for` 循环遍历
  - 类数组只可以用 `for` 遍历（因为类型是 Object）

# 判断数组的方法

数组属于 Object 类型，不能用 typeof 来判断

- 构造函数
	- `arr.constructor === Array // true`
	- `arr instanceof Array // true` 
		但 instanceof 假定只有一个全局执行的环境,如果网页中包含多个框架就会存在不同的全局执行环境，从而存在不同版本的Array构造函数

- 原型链
	- `Array.prototype.isPrototypeOf(arr) // true` 判定Array是不是在arr的原型链中
	- `Object.prototype.toString.call(arr) //[object Array]`

- ES5新增：`Array.isArray(arr) // true`

[JS判断数组的六种方法详解](https://segmentfault.com/a/1190000017790888)

# 类数组

定义：拥有 length 属性，若干索引属性的任意对象

如NodeList、HTML Collections、字符串、arguments

## 类数组对象转数组

- 新建数组，遍历类数组对象，将每个元素放入新数组
- Array.prototype.slice.call(ArrayLike) 或 [].slice.call(ArrayLike)
- Array.from(ArrayLike)
- apply 第二参数传入，调用数组方法
	`Array.prototype.push.apply(null, ArrayLike)`

## Array.from

`Array.from(arrayLike[, mapFunction[, thisArg]])`

`Array.from(someNumbers, value => value * 2)`

- arrayLike：必传参数，想要转换成数组的伪数组对象或可迭代对象
- mapFunction：可选参数，mapFunction(item，index){…} ，在集合中的每个元素上调用函数
- thisArg：可选参数，执行回调函数 mapFunction 时 this 对象，这个参数很少使用

返回一个新的数组实例

```
//从 String 生成数组
Array.from('foo'); // [ "f", "o", "o" ]

//在 Array.from 中使用箭头函数
Array.from([1, 2, 3], x => x + x); // [2, 4, 6]

//序列生成器(指定范围)
function range(end) {
    return Array.from({ length: end }, (_, index) => index);
}
range(4); // => [0, 1, 2, 3]

//数组去重合并
function combine(){
    let arr = [].concat.apply([], arguments);  
    return Array.from(new Set(arr));
}
var m = [1, 2, 2], n = [2,3,3];
console.log(combine(m,n)); // [1, 2, 3]

//填充数组，与 array.fill() 方法相同
const length = 3;
const init = 0;
const result = Array.from({ length }, () => init); //[0, 0, 0]

//array.fill()
const length = 3;
const init   = 0;
const result = Array(length).fill(init);
fillArray2(0, 3); // => [0, 0, 0]
```

参考资料：
[Array.from()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/from)
[Array.from()五个屌炸天的用法](https://www.mybj123.com/4741.html)

# Array、Set 与 Map 的遍历

数组可通过遍历下标来遍历，但 Set 和 Map 没有下标

ES6 引入了 for...of 循环，可遍历 Array、Set、Map

或者使用 forEach() 遍历：
```
var a = ['A', 'B', 'C'];
a.forEach(function (element, index, array) {
    // element指向当前元素的值，index指向当前索引
    alert(element);
});

var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, sameElement, set) {
    alert(element);
});

var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
m.forEach(function (value, key, map) {
    alert(value);
});

```

# 数组拷贝

## 浅拷贝与深拷贝的区别

- 浅拷贝仅仅复制对象的引用，而不是复制对象本身；深拷贝会把复制对象所引用的全部对象的值都复制一遍
- 递归调用浅拷贝就可以实现深拷贝

## 浅拷贝方法

    concat：`[1, 2, 3].concat()` 浅拷贝

    slice：`fn1 = fn.slice()` 浅拷贝

    map：`fn1 = fn.map((x) => x)` 浅拷贝

    filter：`fn1 = fn.filter(() => true)` 浅拷贝

    assign：`fn1 = Object.assign({}, fn)` 浅拷贝

	Array.from：`fn1 = Array.from(fn)` 浅拷贝

    扩展运算符：`fn1 = [...fn]` 浅拷贝

## 深拷贝方法

### 递归调用实现深拷贝
```
function deepCopy(target) {
	if (typeof target === 'object') {
		const newTarget = Array.isArray(target) ? [] : Object.create(null)
		for (const key in target) {
			newTarget[key] = deepCopy(target[key])
		}
		return newTarget
	} else {
		return target
	}
}
```

### 用 assign 方法深拷贝
```
function clone(origin) {
  let originProto = Object.getPrototypeOf(origin);
  return Object.assign(Object.create(originProto), origin);
}
```

# 数组去重
```
//Set去重
function unique (arr) {
	return Array.from(new Set(arr))
}

//Set + 扩展运算符
function unique (arr) {
	return [...new Set(arr)]
}

//for 循环 + splice
function unique (arr) {
	for (let i = 0; i < arr.length; i++) {
		for (let j = i + 1; j < arr.length; j++) {
			if (arr[i] === arr[j]) {
				arr.splice(j, 1)
				j--
			}
		}
	}
	return arr
}

//sort + splice
function unique (arr) {
	arr.sort()
	for (let i = 0; i < arr.length - 1; i++) {
		if (arr[i] === arr[i + 1]) {
			arr.splice(i, 1)
			i--
		}
	}
	return arr
}
```
   
# 数组分片操作

## slice()

`slice(start, end)`

## substr() 与 substring()

`substr(start, length)`

`substring(start, end)`

## slice()、substr() 与 substring() 的区别

## split() 与 join()

`split(separator, limit)` 超出 limit 的字符串会被丢弃

`join(delimiter)` 将数组连接成字符串


# 异步操作

## 实现异步请求的方式

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

## 回调函数 Ajax

Asynchronous JavaScript + XML（异步JavaScript和XML）

原理：利用 XMLHttpRequest 向服务器发送异步请求，并将服务器返回的数据交还给客户端处理，利用 JS 的 DOM 操作去更新页面，在form表单提交的时候，可实现页面不刷新，缺点是没有history(-1)

JSON 和 XML 都用于在 Ajax 模型中打包信息，由于 JSON 更加轻量而且是 JavaScript 的一部分，目前 JSON 的使用比 XML 更普遍

### XMLHttpRequest API （ Ajax 的核心）

### fetch API 

### Server-sent 事件

## 事件监听 addEventListener

## 观察者模式 

## Promise（具体见 ES6 部分）

## async/await

- try catch
await 函数外，包裹 try {}，当 catch (error) 时，捕获错误

- catch
async 函数返回的 Promise，调用 then 方法，然后 catch(error)


## co 库的 generator 函数 



# 正则表达式

## 实现 trim 去除首尾空格

```
//ES5
''.trim()
//正则
''.repalce(/^\s+|\s+$/g, '')
```

# 求dom元素下所有dom元素个数

document.getElementsByTagName('*').length 


# 0.1+0.2 !=== 0.3

因为 js 使用的双精度浮点（64位 = 符号位（1位） + 整数（11位） + 小数（52位）），十进制与二进制之间的转换四舍五入导致出现误差，导致0.1+0.2=0.30000000000000004。和0.3相比较结果为false

最好的方法是设置一个误差范围值，通常称为”机器精度“，而对于Javascript来说，这个值通常是2^-52,而在ES6中,
已经为我们提供了这样一个属性：Number.EPSILON，而这个值正等于2^-52

1. 将其先转换成整数，再相加之后转回小数。具体做法为先乘10相加后除以10.如下图
```
 let x=(0.1*10+0.2*10)/10;
 console.log(x===0.3)
```

2. 使用 toFixed 方法可以指定运算结果的小数点后的指定位数的数字
```
//保留一位小数
// toFixed 方法会将 number 类型转换成 string 类型
let x=parseFloat((0.1+0.2).toFixed(1));
console.log(x===0.3);
```

3. 使用 ES6 新增的 Number.EPSILON 方法，这个方法表示 js 的最小精度，使用这个方法通常只是对 0.1+0.2 是否等于 0.3 做判断，并不像前两种改变 0.1+0.2 的值
```

function equal(a,b){
   return Math.abs(a-b)<Number.EPSILON;
   //相当于把比较的boolean值返回
}
console.log(equal(0.1 + 0.2, 0.3))
```