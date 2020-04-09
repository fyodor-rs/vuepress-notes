## 什么是DOCTYPE及其作用?

> **DOCTYPE是document type(文档类型)的简写，用来说明你用的XHTML或者HTML是什么版本**。浏览器根据定义的DTD来解释页面的标识，并呈现出来。
>
> 在 HTML 4.01 中， 声明需引用 DTD （文档类型声明），因为 HTML 4.01 是基于 SGML （Standard Generalized Markup Language 标准通用标记语言）。DTD 指定了标记语言的规则，确保了浏览器能够正确的渲染内容。
>
> HTML5 不是基于 SGML，因此不要求引用 DTD。

## HTML5语义化与新特性

### 什么是html语义化？

- 表示选择合适的标签便于开发者阅读和写出更优雅的代码

### 为什么使用语义化标签

- 在没有css的情况下，页面整体也会呈现更好的结构效果
- 更有利于用户体验
- 更有利于搜索引擎优化
- 代码结构清晰，方便团队开发和维护

### HTML新特性有哪些？

#### 新特性

- 拖拽释放API
- 语义化更好的内容标签（header，nav，footer，aside，article，section）
- 音频，视频（audio，video）
- 画布（Canvas）API
- 地理API
- 本地离线存储（localstorage，sessionStorage）
- 表单控件（calendar,date,time,email,url,search）
- 新的技术webworker，websocket

#### 移除的元素

- 纯表现的元素：basefont,big center,font,s,strike,tt,;
- 对可用性产生负面影响的元素：frame，frameset，noframes

## 行内元素与块级元素

### 行内元素的特点？

- 元素排在一行
- 只能包含文本或者其他内联元素
- 宽高就是内容宽高，设置宽高无效

> 常见的内联元素标签：a,br,code,em,img,input,span,i,textarea,b

### 块级元素的特点？

- 元素单独占一行
- 元素的宽高都可以设置
- 可以包含内联元素和其他块元素
- 为设置宽度时，默认宽度是它容器的100%

> 常见的块级元素标签：div，p，dl，dt，form，h1~h6,

## meta标签有哪些属性

> 简介: 常用于定义页面的说明，关键字，最后修改日期，和其它的元数据。这些元数据将服务于浏览器（如何布局或重载页面），搜索引擎和其它网络服务。

### charset属性

```html
<meta charset="utf-8" />
<!-- 定义网页文档的字符集 -->
```

### name + content属性

```html
<!-- 网页作者 -->
<meta name="author" content="开源技术团队"/>
<!-- 网页地址 -->
<meta name="website" content="https://sanyuan0704.github.io/frontend_daily_question/"/>
<!-- 网页版权信息 -->
 <meta name="copyright" content="2018-2019 demo.com"/>
<!-- 网页关键字, 用于SEO -->
<meta name="keywords" content="meta,html"/>
<!-- 网页描述 -->
<meta name="description" content="网页描述"/>
<!-- 搜索引擎索引方式，一般为all，不用深究 -->
<meta name="robots" content="all" />
<!-- 移动端常用视口设置 -->
<meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0, user-scalable=no"/>
<!-- 
  viewport参数详解：
  width：宽度（数值 / device-width）（默认为980 像素）
  height：高度（数值 / device-height）
  initial-scale：初始的缩放比例 （范围从>0 到10）
  minimum-scale：允许用户缩放到的最小比例
  maximum-scale：允许用户缩放到的最大比例
  user-scalable：用户是否可以手动缩 (no,yes)
 -->
```

### http-equiv属性

```html
<!-- expires指定网页的过期时间。一旦网页过期，必须从服务器上下载。 -->
<meta http-equiv="expires" content="Fri, 12 Jan 2020 18:18:18 GMT"/>
<!-- 等待一定的时间刷新或跳转到其他url。下面1表示1秒 -->
<meta http-equiv="refresh" content="1; url=https://www.baidu.com"/>
<!-- 禁止浏览器从本地缓存中读取网页，即浏览器一旦离开网页在无法连接网络的情况下就无法访问到页面。 -->
<meta http-equiv="pragma" content="no-cache"/>
<!-- 也是设置cookie的一种方式，并且可以指定过期时间 -->
<meta http-equiv="set-cookie" content="name=value expires=Fri, 12 Jan 2001 18:18:18 GMT,path=/"/>
<!-- 使用浏览器版本 -->
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<!-- 针对WebApp全屏模式，隐藏状态栏/设置状态栏颜色，content的值为default | black | black-translucent -->
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
```

## 常见的浏览器内核

> 浏览器最重要或者说核心的部分是“Rendering Engine”，可大概译为“渲染引擎”，不过我们一般习惯将之称为“浏览器内核”。负责对网页语法的解释（如[标准通用标记语言](https://baike.baidu.com/item/标准通用标记语言/6805073)下的一个应用[HTML](https://baike.baidu.com/item/HTML)、[JavaScript](https://baike.baidu.com/item/JavaScript)）并渲染（显示）网页。 所以，通常所谓的浏览器内核也就是浏览器所采用的[渲染引擎](https://baike.baidu.com/item/渲染引擎/10982158)，渲染引擎决定了浏览器如何显示网页的内容以及页面的格式信息。不同的浏览器内核对网页编写语法的解释也有不同，因此同一网页在不同的内核的浏览器里的渲染（显示）效果也可能不同，这也是网页编写者需要在不同内核的浏览器中测试网页显示效果的原因。

- Trident（常用ie内核）
- Gecko（常用火狐浏览器）
- Presto（opera前内核）
- Webkit（Safari内核，Chrome内核原型）
- Blink（由谷歌和opera开发）

