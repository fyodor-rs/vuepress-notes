## JS数据类型有哪些？

基本数据类型：

- boolean
- null
- undefined
- number
- string
- symbol

引用数据类型：

object（包含普通对象，数组，正则，日期，数学，函数）

## 数据类型检测

### typeof

> typeof可以判断基本数据类型类型，但是对于引用类型，除了函数之外，都会显示'object'

```javascript
typeof 1 // 'number'
typeof '1' // 'string'
typeof undefined // 'undefined'
typeof true // 'boolean'
typeof Symbol() // 'symbol'
typeof [] //'object'
typeof {} //'object'
const f=function(){}
typeof f //'function'
```

### instanceof

> 判断引用类型的常用方式：instanceof。instanceof的原理是基于原型链的查询，只要处于原型链中，判断永远为true。
>
> 但是不能直接判断字面量的基本数据类型，而且左边必须是对象和构造函数，否则会抛出TypeError的错误

```javascript
const Person =function(){}
const p1 =new Person()
p1 instanceof Person //true
var str= 'hello'
str instanceof String //false
```

### Object.prototype.toString.call()

> 所有的数据类型都可以用Object.prototype.toString.call()来检测，而且非常的精准。

```javascript
var a = 123;
console.log(Object.prototype.toString.call(a));    // [object Number]
var b = "string";
console.log(Object.prototype.toString.call(b));    // [object String]
var c = [];
console.log(Object.prototype.toString.call(c));    // [object Array]
var d = {};
console.log(Object.prototype.toString.call(d));    // [object Object]
var e = true;
console.log(Object.prototype.toString.call(e));    // [object Boolean]
var f =  null;
console.log(Object.prototype.toString.call(f));    // [object Null]
var g;
console.log(Object.prototype.toString.call(g));    // [object Undefined]
var h = function () {};
console.log(Object.prototype.toString.call(h));    // [object Function]
var A = new Number();
console.log(Object.prototype.toString.call(A));    // [object Number]
```

### construtor

> construtor使用的也是原型链的知识。返回对创建此对象的数组的引用。
>
> 但是无法检测null和undefined的值。

```javascript
var a = 123;
console.log( a.constructor == Number);    // true
var b = "string";
console.log( b.constructor == String);    // true
var c = [];
console.log( c.constructor == Array);    // true
var d = {};
console.log( d.constructor == Object);    // true
var e = true;
console.log( e.constructor == Boolean);    // true
var f =  null;
console.log( f.constructor == Null);    //  TypeError: Cannot read property 'constructor' of null
var g;
console.log( g.constructor == Undefined);    // Uncaught TypeError: Cannot read property 'constructor' of undefined
var h = function () {};
console.log( h.constructor == Function);    // true
var A = new Number();
console.log( A.constructor == Number);    // true
var A = new Number();
console.log( A.constructor == Object);    // false
```

## 自定义instanceof行为

```javascript
class MyArray {  
  static [Symbol.hasInstance](instance) {
    return Array.isArray(instance);
  }
}
console.log([] instanceof MyArray); // true
//可用于判断基本数据类型
class MyString {  
  static [Symbol.hasInstance](instance) {
    return typeof instance === "string";
  }
}
console.log('123' instanceof MyString); // true
```

## 手写实现instanceof

```javascript
//js里面所有的对象都有proto属性,只有构造函数拥有prototype(原型)属性.
//构造函数的__proto__指向Function的原型,Function的原型的__proto__指向object的原型
//手写instanceof
//推荐使用Object.getPrototype()方法获取对象的__proto__,直接获取兼容性不好
function instance_of(left,right){
  //就是寻找left原型链上的__proto__是否等于right的原型
  //var L=left.__proto__;
  var L=Object.getPrototype(left);  
  const R=right.prototype;
  //排除掉基本数据类型
  if(typeof left !== 'object' || left === null) return false;
  while(true){
     //说明找到顶了
     if(L==null) return false
     if(R==L) return true
     L=Object.getPrototype(L);  
  }
}
```

## Object.is和===的区别

> 主要区别在于Object.is考虑了在严格等于下的几种特殊情况。

```javascript
//源码如下
function is(x, y) {
  if (x === y) {
    //运行到1/x === 1/y的时候x和y都为0，但是1/+0 = +Infinity， 1/-0 = -Infinity, 是不一样的
    return x !== 0 || y !== 0 || 1 / x === 1 / y;
  } else {
    //NaN===NaN是false,这是不对的，我们在这里做一个拦截，x !== x，那么一定是 NaN, y 同理
    //两个都是NaN的时候返回true
    return x !== x && y !== y;
  }
}
```

## 闭包

> 闭包就是当前作用域中存在着对父级作用域的引用。或者：
>
> 闭包是指有权访问另外一个函数作用域中的变量的函数。

```javascript
function f1(){
   var a=1;
   function f2{
    console.log(a); 
   } 
   return f2
}
var x = f1();
x()

```

### 常用的几种情况

- 返回一个函数
- 作为函数参数传递
- 回调函数（定时器，时间监听，ajax请求，等异步任务）
- 立即执行函数

### 解决for循环问题

- 使用立即执行函数，传递参数
- 给定时器传入第三个参数
- 使用es6中的let

## 关于this

> this绑定的几种场景：
>
> - 全局上下文（this默认指向window，严格模式下指向undefined）
> - 直接调用函数（没有执行函数，所以情况和全上下文相同）
> - 对象调用方法（执行了函数，this指向该对象）
> - DOM事件绑定（默认指向绑定的元素，ie默认指向window）
> - new+ 构造函数（构造函数中的this指向实例对象）
> - 箭头函数（箭头函数没有this，默认指向最近的非箭头函数的this，找不到就是window/underfined）
>
> 优先级：new >call,apply,bind>对象.方法>直接调用。

## 常见的数组方法

> `push(value)`将value添加到数组的最后，返回数组长度（改变元素组）

```javascript
let a= [1,2,3,4,5]
let result = a.push(6);
console.log(result);//6返回素组长度
console.log(a);//[1,2,3,4,5,6]原数组被改变
```

> `unshift()`添加元素到数组的开头，返回数组的长度（改变原数组）

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.unshift(1)
console.log(result)           // 6
console.log(a)                // [1, 1, 2, 3, 4, 5]
```

> `pop()`删除数组中最后一个元素，返回被删除元素（改变原数组）

```javascript
let a = [5]
let result = a.pop()
console.log(result)   // 5
console.log(a)        // []
```

> `shift()`删除数组第一个元素，返回被删除元素（改变原数组）

```javascript
let a = [5]
let result = a.shift()
console.log(result)      // 5
console.log(a)           // []
```

> `join（value）`将数组用value连接为字符串（不改变原数组）

```javascript
// Base
let a = [1, 2, 3, 4, 5]
let result = a.join(',')
console.log(result)   // '1,2,3,4,5'
console.log(a)        // [1, 2, 3, 4, 5]

// More
let obj = {
  toString() {
    console.log('调用了toString()方法！')
    return 'a'
  },
  toValue() {
    console.log('toValue()方法！')
    return 'b'
  }
}
result = a.join(obj) // 使用对象时会调用对象自身的toString方法转化为字符串，我们这里重写了toString，从而覆盖了原型链上的toString
// 调用了toString()方法！
console.log(result)   // 1a2a3a4a5
console.log(a)        // [1, 2, 3, 4, 5]

// join的一个相对的方法是字符串的split方法
console.log('1a2a3a4a5'.split('a')) // [1, 2, 3, 4, 5]
```

> `slice(start, end)`返回新数组，包含原数组索引start的值到索引end的值，不包含end(不改变原数组)

```javascript
// Base
let a = [1, 2, 3, 4, 5]
let result = a.slice(2, 4)
console.log(result)   // [3, 4]
console.log(a)        // [1, 2, 3, 4, 5]

// More
console.log(a.slice(1))         // [2, 3, 4, 5]   只有一个参数且不小于0，则从此索引开始截取到数组的末位
console.log(a.slice(-1))        // [5]            只有一个参数且小于0，则从倒数|start|位截取到数组的末位
console.log(a.slice(-1, 1))     // []             反向截取，不合法返回空数组
console.log(a.slice(1, -1))     // [2, 3, 4]      从第一位截取到倒数第一位，不包含倒数第一位
console.log(a.slice(-1, -2))    // []             反向截取，不合法返回空数组
console.log(a.slice(-2, -1))    // [4]            倒数第二位截取到倒数第一位
```

> `splice(index, count, value)`从索引为index处删除count个元素，插入value(改变原数组)

```javascript
// Base
let a = [1, 2, 3, 4, 5]
let result = a.splice(1, 2, 0)
console.log(result)   // [2, 3]
console.log(a)        // [1, 0, 4, 5]

// More
a = [1, 2, 3, 4, 5]
console.log(a.splice(-2))                   // [4. 5]
console.log(a)                              // [1, 2, 3]

a = [1, 2, 3, 4, 5]
console.log(a.splice(-1))                   // [5]
console.log(a)                              // [1, 2, 3, 4]               当参数为单个且小于0时，将从数组的倒数|index|位截取到数组的末位

a = [1, 2, 3, 4, 5]
console.log(a.splice(0))                    // [1, 2, 3, 4, 5]
console.log(a)                              // []

a = [1, 2, 3, 4, 5]
console.log(a.splice(1))                    // [2, 3, 4, 5]
console.log(a)                              // [1]                        当参数为单个且不小于0时，将从当前数代表的索引位开始截取到数组的末位

a = [1, 2, 3, 4, 5]
console.log(a.splice(-1, 2))                // [5]
console.log(a)                              // [1, 2, 3, 4]               从倒数第一位开始截取两个元素，元素不够，只返回存在的元素

a = [1, 2, 3, 4, 5]
console.log(a.splice(0, 2, 'a', 'b', 'c'))  // [1, 2]
console.log(a)                              // ["a", "b", "c", 3, 4, 5]   截取后将value一次填充到数组被截取的位置，value的数量大于截取的数量时，数组中剩余的元素后移
```

> reduce()累计器

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.reduce((accumulator, currentValue, currentIndex, array)=>{
  console.log(accumulator, currentValue, currentIndex, array)
  return accumulator + currentValue
  // 5  1 0 [1, 2, 3, 4, 5]  第一次accumulator的值为reduce第二个参数5, currentValue为数组第一个元素
  // 6  2 1 [1, 2, 3, 4, 5]  第二次accumulator的值为5加上数组a中的第一个值，即是第一次循环时return的值
  // 8  3 2 [1, 2, 3, 4, 5]  同上
  // 11 4 3 [1, 2, 3, 4, 5]  同上 
  // 15 5 4 [1, 2, 3, 4, 5]  同上
}, 5)
console.log(result)   // 20 为最终累计的和
console.log(a)        // [1, 2, 3, 4, 5]

// 无初始值时，accumulator的初始值为数组的第一个元素，currentValue为数组第二个元素
result = a.reduce((accumulator, currentValue, currentIndex, array)=>{
  console.log(accumulator, currentValue, currentIndex, array)
  return accumulator + currentValue
  // 1  2 1 [1, 2, 3, 4, 5]
  // 3  3 2 [1, 2, 3, 4, 5]
  // 6  4 3 [1, 2, 3, 4, 5]
  // 10 5 4 [1, 2, 3, 4, 5]
})
console.log(result)   // 15 为最终累计的和
console.log(a)        // [1, 2, 3, 4, 5]

// Time
a = []
for(let i = 0; i < 10000000; i++) {
  a.push(i)
}
let dateStart = Date.now()
a.reduce((accumulator, currentValue, currentIndex, array)=>{
  return accumulator + currentValue
})
let dateEnd = Date.now()
console.log(dateEnd - dateStart) // 200ms 258ms 257ms 效率与forEach相差也不多，而且比forEach多个累计的功能
```

> [相关资料](https://juejin.im/post/5bb753bd6fb9a05d2272b673#heading-0)

## arguments如何转换成数组

> Array.prototype.slice.call()

```javascript
function sum(a,b){
    let args = Array.prototype.slice.call(arguments);
    console.log(args);
}
```

> Array.from()

```javascript
function sum(a, b) {
  let args = Array.from(arguments);
  console.log(args.reduce((sum, cur) => sum + cur));//args可以调用数组原生的方法啦
}
sum(1, 2);//3
```

> es6展开运算符

```javascript
function sum(a, b) {
  let args = [...arguments];
  console.log(args.reduce((sum, cur) => sum + cur));//args可以调用数组原生的方法啦
}
sum(1, 2);//3
```

> concat+apply

```javascript
function sum(a, b) {
  let args = Array.prototype.concat.apply([], arguments);//apply方法会把第二个参数展开
  console.log(args.reduce((sum, cur) => sum + cur));//args可以调用数组原生的方法啦
}
sum(1, 2);//3
```

## 模拟实现call/apply

> call()方法在使用一个指定的this值，和若干个指定的参数值的前提下调用某个函数或者方法

```javascript
var foo = {
    value：1
}
function bar(){
    console.log(this.value);
}
bar.call(foo);//1
```

> call中this指向了foo，然后执行了bar函数。
>
> 简单来说类似于使用foo对象调用bar方法。

```javascript
//手写实现call函数
Function.prototype.mycall1=function(context){
   //context有时候为null,则默认为window 
   var context = context || window;
   //获取调用的函数(this)赋值给context的某个属性
   context.fn=this;
   //收集参数
   var args = []
   for(var i=1;i<arguments.length;i++){
      args.push('arguments['+i+']');    
   }
   //使用字符串拼接,再使用eval函数
   var result=eval('context.fn('+args+')');
   //删除属性
   delete context.fn;
   //返回执行结果
   return result;
}
//手写实现apply，多了一个数组参数
Function.prototype.myapply = function (context, arr) {
        var context = Object(context) || window;
        context.fn = this;

        var result;
        if (!arr) {
            result = context.fn();
        } else {
            var args = [];
            for (var i = 0; i < arr.length; i++) {
                args.push('arr[' + i + ']');
            }
            result = eval('context.fn(' + args + ')')
        }
        delete context.fn
        return result;
}
```

## JS实现继承的方式

> call实现继承,原理是：在执行context.fn()的过程中Parent内的this指向了new child()。
>
> 缺点：无法继承Parent原型链上的属性。

```javascript
function Parent(){
    this.name = 'parent';
}
function Child(){
    Parent.call(this);
    this.type = 'child'
}
console.log(new Child());
```

> 原型链继承：让Child的原型对象指向Parent的实例，这样就能继承Parent原型链上的属性。
>
> 缺点：修改继承属性(包括构造函数和原型链上的)的值，会影响到其他实例

```javascript
function Parent() {
    this.name = 'parent';
}
Parent.prototype.getNameValue= function(){
    return this.name
}
function Child(){
    this.type="child";
}
Child.prototype=new Parent();
 //如果是Parent.prototype,就会少了构造函数上的属性。
Child.prototype.getTypeValue=function(){
    return this.type
}
//new Child()原型上构造器指向Parent
console.log(new Child());
```

>  组合式继承：结合原型链和构造函数继承的方式，结合了原型链和构造函数的优点。缺点：依然存在原型链上继承的缺点，而且会两次调用new Parent（）

```javascript
function Parent(){
  this.name='parent'
}
Parent.prototype.arr=[1,21,2];
function Child(){
  Parent.call(this);//第二次调用
  this.type='child'
}
Child.prototype= new Parent(). //第一次调用
//new Child()原型上构造器指向Parent
console.log(new Child());
```

> 寄生组合式继承：解决了组合式继承两次调用的缺点。

```javascript
function Parent(){
  this.name='parent'
}
Parent.prototype.arr=[1,21,2];
function Child(){
  Parent.call(this);
  this.type='child'
}
Child.prototype= Parent.prototype 
Child.prototype.constructor=Child;
//类似于
Parent.prototype.constructor=Child；
Child.prototype=Parent.prototype
//书上的写法
function inheritPrototype(subType,superType){
    var prototype = Object(superType.prototype);//创建对象
    prototype.constructor = subType;//增强对象
    subType.prototype =prototype;//指定对象
}
inheritPrototype(Child,Parent)
//new Child()原型上构造器指向Child
console.log(new Child());
```

> 扩展Object.create(Parent.prototype)//创建一个新对象，使得新对象的proto属性指向Parent.prototype

## 执行环境/上下文

> **作用域**：作用域就是变量与函数的可访问范围，即作用域控制着变量与函数的可见性和生命周期。在JavaScript中，变量的作用域有全局作用域和局部作用域两种，局部作用域又称为函数作用域。
>
> **作用域链**：JavaScript上每一个函数执行时，会先在自己创建的`AO`（活动对象）上找对应属性值。若找不到则往父函数的AO上找，再找不到则再上一层的`AO`,直到找到大boss:`window`（全局作用域）。 而这一条形成的“`AO`链” 就是`JavaScript`中的作用域链。
>
> **执行上下文**：执行上下文就是当前 JavaScript 代码被解析和执行时所在环境的抽象概念， JavaScript 中运行任何的代码都是在执行上下文中运行。
>
> 执行上下文三种类型：
>
> - 全局执行上下文
> - 函数执行上下文
> - eval函数执行上下文
>
> 执行上下文创建过程：
>
> - 确定this指向
> - 创建词法环境
> - 创建变量对象

## new的过程

> 模拟实现new的效果

```javascript
function newMethod(ctor,...args){
    if(typeof ctor !== 'function'){
       throw 'newOperator function the first param must be a function';
    }
    //创建一个新对象
    let obj = new Object();
    //对象的__proto__指向构造函数的原型对象。
    obj.__proto__=Object.create(ctor.prototype);
    //改变this指向，继承构造函数的属性，创建实例。
    let res =ctor.apply(obj,...args);
    let isObject = typeof res === 'object' && typeof res !==null;
    let isFunction = typeof res === 'function';
    //如果res是基本数据类型，则返回空对象。
    return isObject || isFunction ? res:obj;
}
```

## bind函数的实现原理

```javascript
if (!Function.prototype.bind) (function(){
  var slice = Array.prototype.slice;
  Function.prototype.bind = function() {
      //保存原函数，获取this的指向
    var thatFunc = this, thatArg = arguments[0];
      //获取参数
    var args = slice.call(arguments, 1);
      //如果调用对象不是函数，则报错
    if (typeof thatFunc !== 'function') {
      // closest thing possible to the ECMAScript 5
      // internal IsCallable function
      throw new TypeError('Function.prototype.bind - ' +
             'what is trying to be bound is not callable');
    }
    return function(){
      //拼接参数 
      var funcArgs = args.concat(slice.call(arguments))
      return thatFunc.apply(thatArg, funcArgs);
    };
  };
})();
```

> 有两次参数传递的过程