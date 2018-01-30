### 模式

* 设计模式
* 编码模式
* 反模式

### 面向对象
首先，`javascript`一切皆对象。

对象的两种类型：

* 原生的（Native）

>[info] 独立与宿主环境的`ECMAScript`实现提供的对象

* 主机的（Host）

>[info] 在主机环境中（如：浏览器）定义的对象

### 原型（prototype）

当我们创建一个函数时，`javascript`默认给它创建了一个对象属性`prototype`。
这个`prototype`的属性值是一个对象，默认只有一个`constructor`的属性，指向所创建的函数本身。

```js
let Apple = function(){
  
}
let app = new Apple();
console.log(app.__proto__.constructor == Apple);//true
```

每个对象都有一个隐藏的属性——`“__proto__”`，这个属性引用了创建这个对象的函数的`prototype`。

### 函数声明和函数表达式

* 表达式

```js
//又名匿名函数
var add = function(a,b){
	return a+b;
}
add.name;// ""
//命名函数表达式，是函数表达式的一种
var add = function add(a,b){
}

add.name; // "add"
```

* 函数声明
```js
function foo(){
	//函数主体
}
foo.name; // "foo"
```

### apply与call

#### apply

带有两个参数，第一个参数是指定将要绑定到该函数内部`this`的一个对象；
第二个参数是一个数组或多个变量参数，这些参数将变成可用于该函数内部的类似数组的`arguments`对象。
如果第一个参数为`null`，那么`this`将指向全局对象。

```js
Function.apply(obj,args)
```

>[info] `apply`方法能劫持另外一个对象的方法，继承另外一个对象的属性.  

eg:

```js
//定义一个函数
function App(name){
  this.name = name;
  this.created();
}

//扩展原型
App.prototype={
  created:function(){
    console.log(`${this.name} has been created!`);
  },
  destroy:function(reason){
    let re = reason || "no reason";
    this.destroy_time = new Date().getTime();
    console.log(`${this.name} has been destroyed at ${this.destroy_time} for ${re}!`);
  }
}

let app = new App('app');
console.log(app.destroy());//app has been destroyed at 1514355097846 for no reason!

let test = {
  name:'test'
}
app.destroy.apply(test,["it is test simple"]);//test has been destroyed at 1514355097847 for it is test simple!
```

**简单概括就是**：

>[danger]  将`app.destroy`的方法，应用到`test`对象上，使得`test`也具有`destroy`的功能。
>换句话说，就是为了改变函数体内部 `this` 的指向


#### call

`call`的用法和功能与`apply`完全一样，只是接收参数不一样。

```js
Function.call(obj,[param1[,param2[,…[,paramN]]]]) 
```
没有参数数量的限制，参数已参数列表的形式传递过来。

#### 应用

* 继承

```js
function Parent(){
  this.name = "parent";
  this.age = 18;
  this.infomation = "this is a girl";
}

let child = {};
Parent.apply(child);
console.log(child.name)// "parent"

```

* 获取数组中的最大值

```js
let array = [1,2,3,3,10,0];
let maxVal = Math.max.apply(null,array);
console.log(maxVal);//10
```

#### 还有个bind

```js
obj.bind(thisObj, arg1, arg2, ...);
```
把obj绑定到thisObj，这时候thisObj具备了obj的属性和方法。与call和apply不同的是，bind绑定后不会立即执行。

```js
function add(j, k){
    return j+k;
}


function sub(j, k){
    return j-k;
}

add.bind(sub, 5, 3); //不再返回8
add.bind(sub, 5, 3)(); //8
```

### Curry化（柯里化）

>[info] 柯里化（Currying），又称部分求值（Partial Evaluation），是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。

一个通用柯里化函数：

```js
let currying = function(fn){
  let stored_args = [].slice.call(arguments,1);
  console.log(stored_args);//截取第一个参数
  return function(){
    let args = stored_args.concat([].slice.call(arguments));
    return fn.apply(null,args);
  }
}

function add(x,y){
  return x+y;
}

let result = currying(add,5)(2);
console.log(result);//7

```

>[warning] `arguments`这个对象还有一个名叫 callee 的属性，该属性是一个指针，指向拥有这个 arguments 对象的函数。
>


