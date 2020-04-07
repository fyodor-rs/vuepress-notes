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