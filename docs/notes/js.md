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

