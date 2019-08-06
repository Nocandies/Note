[TOC]

# JavaScript 笔记

* 单线程

* 解释性

* 预编译

    <boolean string object function undefined number>

## 预编译 

>预编译发生在函数执行的前一刻  

### 四部曲  

```js
function fn(a){
            console.log(a);
            var a = 123;  // 预编译已经声明了变量 只执行a = 123 --> function==>123
            console.log(a);
            function a(){}
            console.log(a);
            var b = function (){}  // 是函数表达式 不是函数声明   现在才赋函数体
            console.log(b);
            function d(){}
        }
        fn(1);
```

1. 创建AO对象  (Activation Object)  (执行期上下文)   /// 全局是GO (Global Object)===window

2. 找形参和变量声明，将变量和形参名作为AO属性名，值为 undefined

3. 将实参值和形参统一

4. 在函数体里面找函数声明，值赋予函数体

   ``` js
   AO {
       a : function a () {},    //   undefine==>1==>function
       b : undefined,
       d : function d () {}
   }
   ```

``` javascript
console.log(a);
var a=123;
//................... <==> 
var a;
console.log(a);
a=123;
```

### 变量

`` arguments[]`` 隐式参数

#### 1.  `imply global`   暗示全局变量：即任何变量，如果变量未经声明就赋值，此变量就为全局(window)===GO所有

``` javascript
a = 10;
//...............<==>
window.a = 10;
```

#### 2.  一切声明的全局变量 ，全是window的属性  

``` js
var a = 123; 
//..............<==>
window.a = 123;
```

## 作用域精解

> 运行期上下文： 当函数执行时，会创建一个称为执行期上下文的内部对象。一个执行期上下文定义了一个函数执行时的环境，函数每次执行时对应的上下文都是独一无二的，所以多次调用一个函数会导致创建多个执行上下文，当函数执行完毕，它所产生的执行上下文被销毁。

> 查找变量 ： 从作用域链的顶端依次向下查找。

> [[scope]]: 每个javascript函数都是一个对象，对象中有这些属性我们可以访问，旦有些不可以，这些属性仅供javascript引擎存取，[[scope]]就是其中一个。
>
> [[scope]]指的就是我们所说的作用域，其中存储了运行期上下文的集合。

> 作用域链： [[scope]]所存储的执行期上下文对象的集合，这个集合呈链式连接，我们把这种链式连接叫做作用域链。

## 闭包

```js
 function a(){
            function b(){
                var b = 234;
            }
            var a = 123;
            b();
        }
        var glob = 100;
        a();
```

![1563759776792](C:\Users\Administrator\Desktop\md\assets\1563759776792.png)

![1563759815150](C:\Users\Administrator\Desktop\md\assets\1563759815150.png)

![1563759841634](C:\Users\Administrator\Desktop\md\assets\1563759841634.png)

![1563759882214](C:\Users\Administrator\Desktop\md\assets\1563759882214.png)

>当内部函数被保存到外部时，将会生成闭包。闭包会导致原有作用域链不释放，造成内存泄漏。

### 闭包的作用

> 实现共有变量  

> 可以做缓存(存储结构)  

> 可以实现封装，属性私有化。

> 模块化开发，防止污染全局变量

## 立即执行函数



``` js
//.........eg:
var num = ( function(a,b,c){
    var d = a+b+c;
    return d;
} (1,2,3) );
```

> 定义 ： 此类函数没有声明，在一次执行过后即释放。适合做初始化工作。

> 只有表达式才能被执行符号执行    /// 函数前加上   +   - ！ 可用()执行

``` js
 function test(a,b,c,d){
           console.log(a+b+c+d)(1,2,3,4);
   }
// 被默认为
function test(a,b,c,d){
           console.log(a+b+c+d)
           
           
           (1,2,3,4);
   }  // 不报错  不执行
```

打印0 1 2 3 4 5 6 7 8 9 

``` js
function test(){
           var arr = [];
           for (var i = 0;i < 10;i++){
               (function (x){  // 这里的x是形参 
                    arr[x] = function(){
                    document.writeln(x + ' ');
                    } 
               }(i));  // 这里的i为实参 0123456789
           }
           return arr;
       }
       var myarr = test();
       for (var j = 0;j < 10;j ++){
           myarr[j]();
       }
```

## 对象

`delete obj.exam`删除obj对象的exam属性

### 对象的创建方法

1. ``` var obj = {}```    plainObject 对象字面量/对象直接量

2. 构造函数

   ​	1)系统自带的构造函数 `Object()`  ``` var obj = new Object();```

   ​	2)自定义       大驼峰式命名规则

   ​	3)``` var obj = Object.creat(原型);```  //Object or null    所以是绝大多数对象最终会继承自	            	    Object.prototype

   #### 构造函数内部原理   
   
   > 1. 在函数体最前面隐式加上 this = {}
   >
   > 2. 执行 this.xxx = xxx;
   > 3. 隐式的返回this

### 包装类

Number()   String()等等

## 原型 (prototype)

> 定义： 原型是function对象的一个属性，它定义了构造函数制造出的对象的公共祖先。通过该构造函数产生的对象，可以继承该原型的属性和方法。原型也是对象。

> 利用原型特点和概念，可以提取共有属性。

> 对象如何查看原型 ->隐式属性-> \__proto__

> 对象如何查看对象的构造函数 -> constructor

``` js
function Person (){
    /*
    var this = {
        __proto__ : Person.prototype     //隐式生成一个__proto__ 属性指向构造函数原型
    }
    */
}
```

### 原型链

## this 

> 函数预编译过程 this —> window
>
> 全局作用域里 this —> window 
>
> call / apply 可以改变函数运行时 this 指向
>
> obj.func();  func()里面的this指向obj

## call / apply

> 作用： 改变 this 指向
>
> 区别 ： 后面传的参数形式不同
>
> > call 需要把实参按照形参个数传进去
> >
> > apply 需要传一个 arguments 数组

## arguments

> arguments.callee 			函数自身

```js
function test(){
    if(arguments.callee == test){}   	//true
}
```

> func.caller						谁叫它、、（ES5以后不让用）



## 继承发展史

> 1. 传统形式  ——>原型链
>
>    >过多的继承了 没用的属性
>
> 2. 借用构造函数
>
>    > 不能继承借用构造函数的原型
>    >
>    > 每次构造函数都要多走一个函数
>
> 3. 共享原型
>
>    > 不能随便改动自己的原型
>
> 4. 圣杯模式

### 共享原型 

```js
Father.prototype.lastName = "Chen";
function Father(){
}
function Son (){
}
Son.prototype = Father.prototype;
var son = new Son();			//son.lastName = 'chen'
var father = new Father();		//father.lastName = 'chen'  实现共享
```

```继承   extends()       inherit()```

```js
function inherit(Target,Origin){
    Target.prototype = Origin.prototype;
}
inherit(Son,Father);
```

### 圣杯模式

```js
function inherit(Target,Origin){
    function F(){};							   //中间函数实现
    F.prototype = Origin.prototype;
    Target.prototype = new F();
    Target.prototype.constuctor = Target;	  //Target.prototype.constuctor 归位
    Target.prototype.uber = Origin.prototype;//方便以后查找Target真正的原型超类uber(super)
}
inherit(Son,Father);
```

## 命名空间  （对象）    

> 管理变量，防止污染全局，适用于模块化开发。

> -----已经不用了，现在用闭包---------

## 属性表示方法 

> ```obj.prop ```        ——>内部发生如下变化
>
> ```obj["prop"]```

```js
for(var prop in obj){
    if(obj.hasOwnProperty(prop)){  //hasOwnProperty()判断是否为自身变量或方法
        console.log(obj.prop);   // undefined  内部实际访问的是obj["prop"]  prop是string
    	console.log(obj[prop]);
    }
}
```

## 克隆

> 遍历对象 	```	for(var prop in obj)```
>
> 1. 判断是不是原始值  ``` typeof()    object ``` 
> 2. 判断是数组还是对象       ```instanceof ``` ```toString ``` ```constructor ```
> 3. 建立相应的数组或对象  
>
> 递归

 ``` js
var obj = {
    name = "abc",
    age : 123,
    card : ['visa','master'],
    wife : {
        name : "bcd",
            son : {
                name : "aaa"
            }
    }
}
var obj1 = {}

function deepClone (origin,target){
    var target = target || {};  //容错
    toStr = Object.prototype.toString,
        arrStr = "[object Array]";
    for (var prop in origin){
        if(origin.hasOwnProperty(prop)){
            if(origin[prop]!=="null" && typeof(origin[prop]) == 'object'){
                if(toStr.call(origin[prop]) == arrStr){
                    target[prop] = [];
                }else{
                    target[prop] = {};
                }
                deepClone(origin[prop],target[prop]);
            }else{
                target[prop] = origin[prop];
            }
        }
    }
    return target;
}
 ```

## 数组

```js
var arr = [10]; 			//表示 一个10
var arr = new Array(10);   	 //表示 10个空值	
```

> 改变原数组 
>
> > push,pop(从后增减),shift,unshift(从前减增),sort(Asci码排序),reverse(逆转),
> >
> > splice()  ```arr.splice(从第几位开始,截取多少长度,在切口处添加新的数据);```

> 不改变原数组
>
> > concat(连接),join("") ——>split,toString,slice(从该位开始截取,截取到该位)
> >
> > split("")<—>join("")可逆

### sort()

```js
var arr = [1,5,4,10,6];
arr.sort(function (a,b){
    a > b ? 1 : -1;
});
```



> 1. 必须写两个形参 
> 2. 看返回值   
>     1. 当返回值为负数时，那么前面的数放在前面
>     2. 当返回值为正数时，那么后面的数放前面
>     3. 当返回值为 0 时，不动

## 类数组    （例如arguments）

> 可以利用属性名模拟数组的特性
>
> 可以动态的增长 length 属性
>
> 如果强行让类数组调用 push 方法，则会根据 length 属性值的位置进行属性的扩充。

> 属性要为索引（数字）属性，必须有 length 属性，最好加上 push 

## try...catch

> Error.name 的六种值对应的信息：
>
> 1. EvalError : eval() 的使用与定义不一致
> 2. RangeError : 数值越界
> 3. ReferenceError : 非法或不能识别的引用数值
> 4. SyntaxError : 发生语法解析错误
> 5. TypeError : 操作数类型错误
> 6. URIError : URI处理函数使用不当

## ES5严格模式

```js
"use strict";   		//es5严格模式启动
```

> 不再兼容es3的一些不规则语法。使用全新的es5规范。
>
> 两种用法
>
> > 全局严格模式
> >
> > 局部函数内严格模式（推荐）
>
> 就是一行字符串，不会对不兼容严格模式的浏览器产生影响。
>
> 不支持 ```with``` ,```arguments.callee```,```func.caller```,变量赋值前必须声明，局部 this 必须被赋值(```Person.call(null/undefined)```赋值什么就是什么)，拒绝重复属性和参数

```js
eval("console.log('abc')");   //打印出'abc'字符串
```

## DOM (Document Object Model)

> 节点的类型 
>
> > 元素节点 —— 1
> >
> > 属性节点 —— 2
> >
> > 文本节点 —— 3
> >
> > 注释节点 —— 8
> >
> > document  —— 9
> >
> > DocumentFragment —— 11

> 节点的四个属性
>
> > ```nodeName```	元素的标签名，以大写形式表示，只读
> >
> > ```nodeValue```	Text 节点或 Comment 节点的文本内容，可读写
> >
> > ```nodeType```	该节点的类型，只读 。 返回数字
> >
> > ```attributes```	Element 节点的属性集合

## Date

### js定时器

> setInterval();   			clearInterval();
>
> setTimeout();			  clearTimeout();

> 全局对象window上的方法，内部函数this指向window



## 获取窗口属性

```js
function getScrollOffset(){
    if(window.pageXOffset){
        return {
            x : window.pageXOffset,
            y : window.pageYOffset
        }
    }else{
        return {
            x : document.body.scrollLeft + document.documentElement.scrollLeft,
            y : document.body.scrollTop + document.documentElement.scrollTop
        }
    }
}
```

### 查看视口的尺寸

 > ``` js\
 > window.innerWidth/innerHeight
 > ```
 >
 > IE9以下不兼容

 > ``` js
 > document.documentElement.clientWidth/clientHeight
 > ```
 >
 > 标准模式下，任意浏览器都兼容

 > ```js
 > document.body.clientWidth/clientHeight
 > ```
 >
 > 适用于怪异模式下的浏览器		```document.compatMode``` 判断是否为怪异模式 返回值为 `CSS1Compat` `BackCompat` 

### 查看元素的几何尺寸

>``` js
>domEle.getBoundingClientRect();
>```
>
>> left , top 代表该元素左上角的 x,y 坐标
>>
>> right , bottom 代表该元素右下角的 x,y 坐标
>
>> 返回的结果不是 “ 实时的 ” 

> ```js 
> dom.offsetWidth,dom.offsetHeight
> ```

### 查看元素的位置

> ```js
> dom.offsetLeft,dom.offsetTop
> ```
>
> 对于无定位父级的元素，返回相对文档的坐标。对于有定位父级的元素，返回相对于最近的有定位的父级的坐标

> ```js 
> dom.offsetParent
> ```
>
> 返回最近的有定位的父级，如无，返回body , `body.offsetParent` 返回null

### 让滚动条滚动

> ```js
> scroll() , scrollTo() 		scrollBy();
> ```
>
> 三个方法功能类似，用法都是将x,y坐标传入。即实现让滚动轮滚动到当前位置。
>
> 区别 ： `scrollBy()` 会在之前的数据基础之上做累加。





















## 底层运行

``` js
document.write(obj);
```

是先执行``` obj.toString()```,若```obj.prototype```为null则没有```toString()```方法。无法执行并报错。

------------------------------------------------------------

```js
0.14 * 100;// 返回结果为  14.000000000000002
```

是因为 js 精确度不够。一般采用取整方式处理小数。

> js 可计算范围为小数点前后16位

``` js
Math.ceil(123.234);     //    124   向上取整
Math.floor(123.999);    //    123   向下取整
```

-----------------------

``` js
function test (){
}
test();   //test.call(); 内部访问了call方法
```

------------------

> JavaScript 中各种类型的变量在 if 条件中是 true 还是false ??
>
> 如果操作数是一个对象，返回true
> 如果操作数是一个空字符串，返回false
> 如果操作数是一个非空字符串，返回true
> 如果操作数是数值0，返回false
> 如果操作数是任意非0数值（包括Infinity），返回true
> 如果操作数是null，返回false
> 如果操作数是NaN，返回false
> 如果操作数是undefined，返回false
>
> 基本上 undefined、null、NaN、0 和 false 本身都是 false

```js
NaN == NaN //   false 
null == undefined   // true 
```





























## 思考问题

> 如何实现链式调用模式 （模仿jquery)
>
> 例如 ： ``` obj.eat().smoke().drink().eat().sleep();```

在函数内最后添加  ``` return this;```

-----------------------





































## 工具类js代码

### 类型区分

```js
function type(target){
    var typeStr = typeof(target),
        toStr = Object.prototype.toString,
        objStr = {
            "[object Object]" : "object - Object",
            "[object Array]" : "array - Object",
            "[object Number]" : "number - Object",
            "[obect Boolean]" : "boolean - Object",
            "[object String]" : "string - Obeject"
        };
    if(target === null){
        return 'null';
    }
    if(typeof(target) == "object"){
        var str = Object.prototype.toString.call(target);
        return objStr[str];
    }else {
        return typeof target;
    }
}
```

### 数组去重

```js
Array.prototype.unique = function (){
    var temp = {},
        arr = [],
        len = this.length;
    for (var i = 0 ; i < len ; i ++ ){
        if (!temp[this[i]]){
            temp[this[i]] = "a";
            arr.push(this[i]);
        }
    }
    return arr;
}
```

### 获取滚动条偏移量

```js
function getScrollOffset(){
    if(window.pageXOffset){
        return {
            x : window.pageXOffset,
            y : window.pageYOffset
        }
    }else{
        return {
            x : document.body.scrollLeft + document.documentElement.scrollLeft,
            y : document.body.scrollTop + document.documentElement.scrollTop
        }
    }
}
```

返回浏览器视口尺寸

```js
function getViewportOffset(){
    if(window.innerWidth){
        return {
            w : window.innerWidth,
            h : window.innerHeight
        }
    }else{
        if(document.compatMode === "BackCompat"){
            return {
                w : document.body.clientWidth,
                h : document.body.clientHeight
            }
        }else{
            return {
                w : document.documentElement.clientWidth,
                h : document.documentElement.clientHeight
            }
        }
    }
}
```





















## ？？？

\__proto__ 和prototype 

> ```__proto__```是每个对象都有的一个属性，而prototype是函数才会有的属性!!!
> 使用Object.getPrototypeOf()代替```__proto__```

> 对象具有属性`__proto__`，可称为隐式原型，一个对象的隐式原型指向构造该对象的构造函数的原型，这也保证了实例能够访问在构造函数原型中定义的属性和方法。

![20161208183943943](C:\Users\Administrator\Desktop\md\assets\20161208183943943.png)

---------------

重写Array.sort()

