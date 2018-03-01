### 自运行闭包

```js
(function (global, factory) {
	typeof exports === 'object' && typeof module !== 'undefined' ? module.exports = factory() :
	typeof define === 'function' && define.amd ? define(factory) :
	(global.Vue = factory());
}(this,(function(){})));
```
闭包采用`UMD`的常规写法，首先判断是否支持`Node.js`模块格式（`exports`或`module`是否存在），存在则使用`Node.js`模块格式；再判断是否支持`AMD`（`define`是否存在），存在则使用`AMD`方式加载模块。前两个都不存在，则将模块公开到全局（`window`或`global`）。

### 先从上往下捋

#### 声明严格模式

```js
'use strict';
```
[严格模式的目的](http://www.ruanyifeng.com/blog/2013/01/javascript_strict_mode.html) :

*  消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为;
* 消除代码运行的一些不安全之处，保证代码运行的安全；
* 提高编译器效率，增加运行速度；
* 为未来新版本的Javascript做好铺垫。

#### 一些自定义函数

```js
var emptyObject = Object.freeze({});
```
冻结一个空对象，目的估计是避免操作空对象从而污染对象，全程用来对象的比较判断。

<div class="cline"></div>

```js
function isUndef (v) {
  return v === undefined || v === null
}

function isDef (v) {
  return v !== undefined && v !== null
}

//是否是true
function isTrue (v) {
  return v === true
}

//是否是false
function isFalse (v) {
  return v === false
}

//查询值是否是原始的数据类型，这里应该不包含对象类
function isPrimitive (value) {
  return (
    typeof value === 'string' ||
    typeof value === 'number' ||
    // $flow-disable-line
    typeof value === 'symbol' ||
    typeof value === 'boolean'
  )
}

//判断是否是对象类型了
function isObject (obj) {
  return obj !== null && typeof obj === 'object'
}
```
>[info] these helpers produces better vm code in JS engines due to their explicitness and function inlining

一些功能性函数，因为这些函数的明确性和内联关系，可以更好的在`js`引擎中生成虚拟机代码。

<div class="cline"></div>

```js
var _toString = Object.prototype.toString;

//参考链接：https://www.cnblogs.com/libin-1/p/5902860.html
function toRawType (value) {
  return _toString.call(value).slice(8, -1)
}

/**
 * Strict object type check. Only returns true
 * for plain JavaScript objects.
 */
function isPlainObject (obj) {
  return _toString.call(obj) === '[object Object]'
}

function isRegExp (v) {
  return _toString.call(v) === '[object RegExp]'
}
```

>[info] Get the raw type string of a value e.g. [object Object]

这里解释说明下，什么是`plain javascript objects`?
即：纯粹的对象，通过`{}`或`new Object`创建的对象。

##### 关于`toRawType`的几点说明

1. 为什么使用`Object.prototype.toString.call()`?

目的是获取对象原型的`toString`，防止重写`toString`方法。

2. 原理是什么？

>[info] ECMAScript 3
>在ES3中,Object.prototype.toString方法的规范如下:
>115.2.4.2 Object.prototype.toString()
>在toString方法被调用时,会执行下面的操作步骤:
>1. 获取this对象的[[Class]]属性的值.
>2. 计算出三个字符串"[object ", 第一步的操作结果Result(1), 以及 "]"连接后的新字符串.
>3. 返回第二步的操作结果Result(2).

<div class="cline"></div>

```js
/**
 * Check if val is a valid array index.
 */
function isValidArrayIndex (val) {
  var n = parseFloat(String(val));
  return n >= 0 && Math.floor(n) === n && isFinite(val)
}

/**
 * Convert a value to a string that is actually rendered.
 */
function toString (val) {
  return val == null
    ? ''
    : typeof val === 'object'
      ? JSON.stringify(val, null, 2)
      : String(val)
}

/**
 * Convert a input value to a number for persistence.
 * If the conversion fails, return original string.
 */
function toNumber (val) {
  var n = parseFloat(val);
  return isNaN(n) ? val : n
}
```
一些转换操作，代码看上去很严谨和简洁，学习了！

冷知识，`JSON.stringify`的参数：

>[info] 语法：JSON.stringify(value[, replacer [, space]])

* `value`
	将要序列化成 一个JSON 字符串的值。
    
* `replacer` 可选
	如果该参数是一个函数，则在序列化过程中，被序列化的值的每个属性都会经过该函数的转换和处理；如果该参数是一个数组，则只有包含在这个数组中的属性名才会被序列化到最终的 JSON 字符串中；如果该参数为null或者未提供，则对象所有的属性都会被序列化；关于该参数更详细的解释和示例，请参考使用原生的 JSON 对象一文。
    
* `space` **可选**
	指定缩进用的空白字符串，用于美化输出（pretty-print）；如果参数是个数字，它代表有多少的空格；上限为10。该值若小于1，则意味着没有空格；如果该参数为字符串(字符串的前十个字母)，该字符串将被作为空格；如果该参数没有提供（或者为null）将没有空格。


<div class="cline"></div>

```js
/**
 * Make a map and return a function for checking if a key
 * is in that map.
 */
function makeMap (
  str,
  expectsLowerCase
) {
  var map = Object.create(null);
  var list = str.split(',');
  for (var i = 0; i < list.length; i++) {
    map[list[i]] = true;
  }
  return expectsLowerCase
    ? function (val) { return map[val.toLowerCase()]; }
    : function (val) { return map[val]; }
}

/**
 * Check if a tag is a built-in tag.
 */
var isBuiltInTag = makeMap('slot,component', true);

/**
 * Check if a attribute is a reserved attribute.
 */
var isReservedAttribute = makeMap('key,ref,slot,slot-scope,is');

```

这段是将字符串转为`map`集合，并返回函数，检查`key`值是否存在，具体这块代码的作用稍后分析。

