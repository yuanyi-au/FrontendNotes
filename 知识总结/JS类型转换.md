- [转换结果](#转换结果)
  - [原始值转布尔](#原始值转布尔)
  - [原始值转数字](#原始值转数字)
  - [对象转布尔值](#对象转布尔值)
  - [对象转字符串和数字](#对象转字符串和数字)
- [转换方法](#转换方法)
  - [ToPrimitive() 方法](#toprimitive-方法)
  - [JSON.stringify()](#jsonstringify)
  - [typeof属性](#typeof属性)
- [实例](#实例)


# 转换结果

## 原始值转布尔

```
console.log(Boolean()); // false
console.log(Boolean("")); // false
console.log(Boolean(undefined)); // false
console.log(Boolean(null)); // false
```

## 原始值转数字

```
console.log(Number(undefined)); // NaN
console.log(Number(null)); // 0
console.log(Number("123 123")); // NaN
console.log(Number("3 abc")); // NaN
console.log(parseInt("3 abc")) // 3
```

## 对象转布尔值

所有对象都转换为true

## 对象转字符串和数字

```
console.log([1, 2, 3].toString()); // 1,2,3
console.log(({}).toString()); // [object Object]
console.log([].toString()); // ""
console.log((new Date(2010, 0, 1)).toString()); // Fri Jan 01 2010 00:00:00 GMT+0800 (CST)
```

# 转换方法

## ToPrimitive() 方法

`ToPrimitive(input, PreferredType)`

输入一个值，返回基本类型值

如果输入为基本类型值，直接返回，否则：

- 根据需要转换的类型先后调用 valueOf() 和 toString()
- 都不成功则报错

## JSON.stringify()

将 JavaScript 值转换为 JSON 字符串

## typeof属性

typeof NaN; //"number"

注意：NaN 是唯一一个非自反的值 NaN===NaN; //false


# 实例

```
typeof [{x:1}] //
```

```
[] == ![] //true
{} == !{} // false
![] == '' //true 两者都可以转换为false
false == null //false
undefined == null //true
undefined === null //false
```

```
console.log(Object.prototype.toString.call("jerry"));//[object String]

console.log(Object.prototype.toString.call(12));//[object Number]

console.log(Object.prototype.toString.call(true));//[object Boolean]

console.log(Object.prototype.toString.call(undefined));//[object Undefined]

console.log(Object.prototype.toString.call(null));//[object Null]

console.log(Object.prototype.toString.call({name: "jerry"}));//[object Object]

console.log(Object.prototype.toString.call(function(){}));//[object Function]

console.log(Object.prototype.toString.call([]));//[object Array]

console.log(Object.prototype.toString.call(new Date));//[object Date]

console.log(Object.prototype.toString.call(/\d/));//[object RegExp]

function Person(){};
console.log(Object.prototype.toString.call(new Person));//[object Object]
```