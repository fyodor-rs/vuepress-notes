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

> 判断引用类型的常用方式：instanceof。instanceof的原理是基于原型链的查询，只要处于原型链中，判断永远为true。但是不能直接判断字面量的基本数据类型。

```javascript
const Person =function(){}
const p1 =new Person()
p1 instanceof Person //true
var str= 'hello'
str instanceof String //false
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

## 执行上下文

> [链接](https://www.cnblogs.com/gaosirs/p/10569973.html)
>
> [链接1](https://juejin.im/post/5c64d15d6fb9a049d37f9c20#heading-16)

## 作用域和作用域链

> [链接](https://www.cnblogs.com/gaosirs/p/10569973.html)

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



## arguments如何转换成数组

> Array.prototype.slice.call()

```
function sum(a,b){
    let args = Array.prototype.slice.call(arguments);
    console.log(args);
}
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
    Parent1.call(this);
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
//new Child()原型上构造器指向Parent
console.log(new Child());
```

> 扩展Object.create(Parent.prototype)//创建一个新对象，使得新对象的proto属性指向Parent.prototype

## 深入了解闭包

> 闭包是指有权访问另一个函数作用域中的变量的函数。
>
> 闭包只能取得包含函数中任何变量的最后一个值

   

