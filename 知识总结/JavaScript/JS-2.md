- [JS-2](#js-2)
	- [数组拷贝](#数组拷贝)
		- [浅拷贝与深拷贝的区别](#浅拷贝与深拷贝的区别)
		- [拷贝方法](#拷贝方法)
			- [递归调用实现深拷贝](#递归调用实现深拷贝)
			- [用 assign 方法深拷贝](#用-assign-方法深拷贝)
	- [数组去重](#数组去重)
	- [判断一个对象是不是数组 Array](#判断一个对象是不是数组-array)
	- [分片操作](#分片操作)
		- [slice()](#slice)
		- [substr() 与 substring()](#substr-与-substring)
		- [slice()、substr() 与 substring() 的区别](#slicesubstr-与-substring-的区别)
		- [split() 与 join()](#split-与-join)
	- [正则表达式](#正则表达式)
		- [实现 trim 去除首尾空格](#实现-trim-去除首尾空格)
	- [求dom元素下所有dom元素个数](#求dom元素下所有dom元素个数)
	- [0.1+0.2 !=== 0.3](#0102--03)


# JS-2

## 数组拷贝

### 浅拷贝与深拷贝的区别

- 浅拷贝仅仅复制对象的引用，而不是复制对象本身；深拷贝会把复制对象所引用的全部对象的值都复制一遍
- 递归调用浅拷贝就可以实现深拷贝

### 拷贝方法

    concat：`[1, 2, 3].concat()` 浅拷贝

    slice：`fn1 = fn.slice()` 浅拷贝

    map：`fn1 = fn.map((x) => x)` 浅拷贝

    filter：`fn1 = fn.filter(() => true)` 浅拷贝

    assign：`fn1 = Object.assign({}, fn)` 浅拷贝

    扩展运算符：`fn1 = [...fn]` 浅拷贝

#### 递归调用实现深拷贝

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

#### 用 assign 方法深拷贝

```
function clone(origin) {
  let originProto = Object.getPrototypeOf(origin);
  return Object.assign(Object.create(originProto), origin);
}
```

## 数组去重

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

## 判断一个对象是不是数组 Array

- isPrototypeOf
```
 function isArray(o) {
	return Array.prototype.isPrototypeOf(o)
}
```

- instanceof

- isPrototypeOf

- Object.prototype.toString
```
function isArray(o) {
  return Object.prototype.toString.call(o) === '[object Array]'
}
```
   
## 分片操作

### slice()

`slice(start, end)`

### substr() 与 substring()

`substr(start, length)`

`substring(start, end)`

### slice()、substr() 与 substring() 的区别

### split() 与 join()

`split(separator, limit)` 超出 limit 的字符串会被丢弃

`join(delimiter)` 将数组连接成字符串


## 正则表达式

### 实现 trim 去除首尾空格

```
//ES5
''.trim()
//正则
''.repalce(/^\s+|\s+$/g, '')
```

## 求dom元素下所有dom元素个数

document.getElementsByTagName('*').length 


## 0.1+0.2 !=== 0.3

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