## 一、什么是模块化

模块化是一种规范，解决了以前JavaScript开发过程当中的一些常见的问题:

> 全局变量污染，变量重名，文件依赖顺序。

## 二、模块化的发展历程

### 2.1、CommonJS

> CommonJS 的出发点： JS 没有完善的模块系统，标准库较少，缺少包管理工具。（虽然这些东西，在后端语言中已经是早就被玩的不想玩的东西了） 伴随着 NodeJS 的兴起，能让 JS 可以在任何地方运行，特别是服务端，以达到 也具备 Java、C#、PHP这些后台语言具备开发大型应用的能力，所以 CommonJS 应运而生。

#### 2.1.1、CommonJS常见规范

- 一个文件就是一个模块，拥有单独的作用域
- 普通方式定义的变量、函数、对象都属于该模块内
- 通过require来加载模块
- 通过exports和module.exports来暴露模块中的内容

> export和module.exports的区别：1.当exports和module.exports同时存在的时候，module.exports会盖过exports 2、本质上exports=module.exports={}，也就是exports是对module.exports的引用。

#### 2.1.2、CommonJS加载、作用域

> 所有代码都运行在模块作用域，不会污染全局作用域；模块可以多次加载，但只会在第一次加载的时候运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果；模块的加载顺序，按照代码的出现顺序是同步加载的；CommonJS 输入的是被输出值的拷贝，不是引用。

### 2.2、ES6模块

> export用于对外输出本模块（一个文件可以理解为一个模块）变量的接口；
>
> import用法：`import {fn} from './xxx/xxx'` ( export 导出方式的 引用方式 ) `import fn from './xxx/xxx1'` ( export default 导出方式的 引用方式 ，可以不用在意导出模块的模块名)

#### 2.2.1、es6模块和CommonJS的区别

> CommonJS模块输出的是一个值的拷贝，es6模块输出的是值的引用（commonjs的内部变化不会影响这个值）。
> CommonJS模块是运行时加载，es6模块是编译时输出的接口。
> CommonJS加载的是一个对象，该对象是运行时生成；es6模块不是对象，对外接口是代码静态解析就完成。
> ES6 模块之中，顶层的this指向undefined；CommonJS 模块的顶层this指向当前模块。

### 2.3、AMD的RequireJS

> Asynchronous Module Definition，中文名是异步模块。它是一个在浏览器端模块化开发的规范，由于不是js原生支持，使用AMD规范进行页面开发需要用到对应的函数库，也就是大名鼎鼎的RequireJS，实际上AMD是RequireJS在推广过程中对模块定义的规范化的产出。

> requireJS主要解决的问题：

- 1、 多个js文件可能有依赖关系，被依赖的文件需要早于依赖它的文件加载到浏览器。
- 2、 js加载的时候浏览器会停止页面渲染，加载文件愈多，页面失去响应的时间愈长。
- 3、 异步前置加载！（什么意思？ 后面我们在原理那个章节进行介绍）

```javascript
// 定义模块
define(['myModule'],() => {
  var name = 'Byron';
  function printName(){
     console.log(name);
}
  return {
     printName:printName
   }
})

// 加载模块--第二个函数为回调函数
require(['myModule'],function(my){
   my.printName();
})
```

### 2.4、CMD 的 SeaJS

```javascript
/*define(id, deps, factory)

因为CMD推崇一个文件一个模块，所以经常就用文件名作为模块id；
CMD推崇依赖就近，所以一般不在define的参数中写依赖，而是在factory中写。

factory有三个参数：
function(require, exports, module){}

一，require
require 是 factory 函数的第一个参数，require 是一个方法，接受 模块标识 作为唯一参数，用来获取其他模块提供的接口；

二，exports
exports 是一个对象，用来向外提供模块接口；

三，module
module 是一个对象，上面存储了与当前模块相关联的一些属性和方法。

demo*/
// 定义模块  myModule.js
define(function(require, exports, module) {
  var $ = require('jquery.js')
  $('div').addClass('active');
});

// 加载模块
seajs.use(['myModule.js'], function(my){

});
```

#### 2.4.1、AMD 的 RequireJS 和 CMD 的 SeaJS 的差异

> AMD推崇依赖前置，在定义模块的时候就要声明其依赖的模块. CMD推崇就近依赖，只有在用到某个模块的时候再去require.

AMD和CMD最大的区别是对依赖模块的执行时机处理不同，注意不是加载的时机或者方式不同。很多人说requireJS是异步加载模块，SeaJS是同步加载模块，这么理解实际上是不准确的，其实加载模块都是异步的，只不过AMD依赖前置，js可以方便知道依赖模块是谁，立即加载，而CMD就近依赖，需要使用把模块变为字符串解析一遍才知道依赖了那些模块，这也是很多人诟病CMD的一点，牺牲性能来带来开发的便利性，实际上解析模块用的时间短到可以忽略。