# JavaScript语法

## 一、用法

与CSS相似，有内部JS、外部JS、直接在HTML标签中

* 内部JS：在HTML中使用`<script></script>`标签
```html
<script>
	//JS语句
</script>
```

* 外部JS：写在.JS文件中  在HTML中引入外部JS文件
```html
<script src="URL" type="text/javascript" >
	//如果script标签从外部引入JS，就不要在此script中写其他JS语句
</script>
```

* 直接在标签中
`javascript:JS语句`		与HTML的标签紧密连接 一般与事件绑定
例如：onclick单击事件，触发alert 警告框
```html
<button onclick="javascript:alert('提交成功！')">提交按钮</button>
```

## 二、ECMAScript(语法标准)

JavaScript 的版本由 **ECMAScript（简称 ES）** 标准定义

| **ES5**        | 2009 年 | `Object.defineProperty`, `Array.prototype.forEach`, 严格模式 (`'use strict'`) |      |                           |
| -------------- | ------- | ------------------------------------------------------------ | ---- | ------------------------- |
| **ES6/ES2015** | 2015 年 | 箭头函数 (`=>`), 模板字符串，解构赋值，`let`/`const`, 类 (`class`), Promise, 模块 (`import`/`export`) |      |                           |
| ES2016         | 2016 年 | `async/await`（基于 Promise）, `Array.prototype.includes`, 指数运算符 (`**`) |      |                           |
| ES2017         | 2017 年 | `Object.values()`, `Object.entries()`, 字符串填充 (`padStart`/`padEnd`) |      |                           |
| ES2018         | 2018 年 | 异步迭代 (`for await...of`), 展开语法 (`...`) 支持对象       |      |                           |
| ES2019         | 2019 年 | `Array.prototype.flat()`, `String.prototype.trimStart()`/`trimEnd()` |      |                           |
| ES2020         | 2020 年 | `nullish` 合并运算符 (`??`), 可选链 (`?.`), `BigInt`, `Promise.allSettled` |      |                           |
| ES2021         | 2021 年 | 逻辑赋值运算符 (`&&=`, `                                     |      | =`, `??=`), `Promise.any` |
| **ES2022**     | 2022 年 | 类私有属性 (`#`), 顶层 `await`, `Object.hasOwn()`            |      |                           |
| **ES2023**     | 2023 年 | 数组和字符串的新方法 (`toReversed`, `toSorted`, `toSpliced` 等) |      |                           |
| **ES2024**     | 2024 年 | 装饰器 (`@decorator`), 数组分组 (`Array.prototype.group`), 更多... |      |                           |



JavaScript是大小写字母区分严格的语言，语法与JAVA语言相似

### 1.变量
数据类型
undefined			没有初始值（未赋值）
null					  空值 与undefined相等
number

```js
var 变量名=变量值; （老版）
let 变量名=变量值; （ES6）
const 常量名=常量值;
```

### 2.语法约定

与JAVA语言相似

### 3.输入/输出

与JAVA语言相似

### 4.注释

与JAVA语言相似

### 5.核心语法

与JAVA语言相似

### 6.控制语句

与JAVA语言相似

### 7.数据类型

与JAVA语言相似

### 8.数组

与JAVA语言相似

### 9.运算符号

与JAVA语言相似



## 三、DOM(文档对象模型)
DOM 为文档提供了结构化表示  并定义了如何通过脚本来访问文档结构  为了能让js操作html元素而制定的一个规范

### 1.DOM对象的属性
innerHTML		双闭合标签里面的内容（包含标签）
innerText		双闭合标签里面的内容（不包含标签）
<--节点（Node）：构成 HTML 网页的最基本单元-->
网页中的每一个部分（html标签、属性、文本、注释、整个文档等）都可以称为是一个节点

### 2.节点的属性
节点类型		nodeType == 1 元素节点（标签） / 2 属性节点  / 3 文本节点
节点名		nadeName
节点值		nadeValue

文档节点（文档）：整个 HTML 文档。整个 HTML 文档就是一个文档节点
元素节点（标签）：HTML标签
属性节点（属性）：元素的属性
文本节点（文本）：HTML标签中的文本内容（包括标签之间的空格、换行）

### 3.获取节点
通过标签id属性值		（唯一）		document.getElementById()
通过标签name属性值	（数组）		document.getElementsByName()
通过标签名			（数组）		document.getElementsByTagName()
通过类名				（数组）		document.getElementsByClassName()
数组一般先遍历再使用（特殊：数组只有一个值，直接用 [0] 获取

父子兄节点之间访问关系
父节点			兄弟节点					子节点				所有子节点
parentNode		nextSibling				firstChild			childNodes
				nextElementSibling		firstElementChild	children
				previousSibling			lastChild				
				previousElementSibling	lastElementChild		

### ★4.DOM的节点操作

创建节点								let 新标签（节点） = document.createElement("标签名");
插入节点（在父节点最后）				父节点.appendChild(新节点);
插入节点（在父节点中的参考节点之前）	父节点.appendChild(新节点,参考节点);
删除节点								父节点.removeChild(子节点);
		#补充：自己删自己				节点.parentNode.removeChild(节点);
复制节点								要复制的节点.clonNode([false]/true)
[false]:只复制节点自身，不复制子节点
​										true:	既复制节点自身，也复制子节点

设置节点属性
获取节点属性				节点.属性; / 节点[属性名] / 节点.getAttribute（推荐）
							前两个是直接操作标签，最后一个是把标签作为DOM节点
设置节点属性				节点.属性 = "值"; / 节点[属性名] = 值"; / 节点.setAttribute("属性名","属性值")
删除节点属性				节点.removeAttribute(属性名);

### ★5.事件
绑定事件的方式
* 直接绑定匿名函数			
```js
事件源.事件 = function(){事件驱动程序}
```

* 先单独定义函数，再绑定
```js
事件源.事件 = fun
function fun(){事件驱动程序}
```

* 行内绑定（标签就是事件源）	
```js
<标签 事件 = "fun()" />
function fun(){事件驱动程序}
```

>常用事件
onclick			鼠标单击
ondblclick		鼠标双击
onkeyup			按下并释放键盘上的一个键时触发
onchange		文本内容或下拉菜单中的选项发生改变
onfocus			获得焦点，表示文本框等获得鼠标光标
onblur			失去焦点，表示文本框等失去鼠标光标
onmouseover		鼠标悬停
onmouseout		鼠标移出
onload			网页文档加载事件
onunload		关闭网页时
onsubmit		表单提交事件
onreset			重置表单时

## 四、BOM（浏览器对象模型）
