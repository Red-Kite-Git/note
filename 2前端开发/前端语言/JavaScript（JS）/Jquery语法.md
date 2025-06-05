# Jquery

Jquery是对原生JS的封装，提供了许多快捷的语句与函数



## 一、选择器

### 1.ID选择器
```js
$("#标签ID属性值")
```

### 2.类选择器
```js
$(".标签CLASS属性值")
```

### 3.元素选择器
```js
$("标签元素")
```

### 4.组合选择器
```js
$("元素1, 元素2, 元素3")
```

### 5.属性选择器
```js
$("标签元素[属性名='属性值']")
```



## 二、事件与方法

### 1.事件函数

#### <1>页面文档结构加载完成事件
```javascript
$(document).ready(function(){});
$(function(){});
```
> 对比原生JS: `window.onload`事件，等待页面所有内容都加载完成以后再执行

#### <2>元素获得焦点事件
```js
$(元素).focus(function () { });
```

#### <3>元素失去焦点事件
```js
$(元素).blur(function () { });
```

#### <4>鼠标悬停事件
```js
$(元素).hover(function () { }, [function () { }]);
//鼠标移入执行第一个函数，鼠标移出执行第二个函数	[没有第二函数则不执行任何操作]
```
>对比原生JS: 
>`mouseover()`方法 鼠标悬停事件
>`mouseout()`方法 鼠标移除事件

#### <5>键盘监听事件
```js
// keydown方法 键盘按下事件
$(元素).keydown(function () { })

// keyup方法 键盘弹起事件
$(元素).keyup(function () { })

// keypress方法 键盘按下打印出字符
$(元素).keypress(function () { })
```


### 2.方法函数
#### <1>css([属性,值])方法
设置元素的样式/获取元素的样式
```js
$(元素).css(属性,值);   $(元素).css({属性:值,属性:值});     $(元素).css();
```

#### <2>val()方法
获取或设置元素的值
```js
$(元素).val(文本);        $(元素).val()
```

#### <3>addClass()方法
追加一个或多个类 
```js
$(元素).addClass(类名);
```

#### <4>removeClass()方法
移除一个或多个类
```js
$(元素).removeClass(类名);
```

#### <5>toggleClass()方法
切换类
```js
$(元素).toggle(fn1,fn2,fn3);
    //toggle()方法切换显示和隐藏
$(元素).toggleClass(类名);  
```

#### <6>hasClass()方法
判断是否存在某个类
```js
$(元素).hasClass(类名)
```

#### <7>html()方法
不对HTML代码解析 直接将HTML代码替换到指定元素中或者获取元素中HTML代码
```js
$(元素).html(标签/内容);     $(元素).html();
```

#### <8>text()方法
对HTML代码进行解析转换为文本 然后替换到指定元素中或则获取元素中文本
```js
$(元素).text(标签/内容);     $(元素).text();
```

#### <9>append()方法
在被选元素的结尾追加内容
```js
$(元素).append(标签/内容);
```

#### <10>prepend()方法
在被选元素的子元素开头插入指定内容
```js
$(元素).append(标签/内容);
    //append()方法在被选元素的子元素结尾插入指定内容
$(元素).prepend(标签/内容);
```

#### <11>before()方法
在被选元素之前插入指定内容
```js
$(元素).after(标签/内容);
    //after()方法在被选元素之后插入指定内容
$(元素).before(标签/内容);
```

#### <12>empty()方法
从被选元素中移除所有子元素
```js
$(元素).remove();
    //remove()方法从被选元素中移除全部元素（包括子元素）
$(元素).empty();
```

#### <13>replaceAll()方法
用被选元素替换指定的元素
```js
$(元素).replaceWith(标签/内容);
    //replaceWith()方法用指定的内容替换被选元素
$(元素).replaceAll(标签/内容);
```

#### <14>clone()方法
复制被选元素，包括所有后代元素和文本内容
```js
$(元素).clone(true);
```

#### <15>attr()方法
设置或返回被选元素的属性和值
```js
$(元素).attr(属性:值);
$(元素).attr({属性:值,属性:值});  
$(元素).attr();
```

#### <16>removeAttr()方法
从被选元素移除属性
```js
$(元素).removeAttr("属性");
```

#### <17>index()方法
返回指定元素相对于其同级元素的索引位置（从 0 开始计数）
```js
$(元素).index()
```

#### <18>获取按键的keyCode
```js
event.keyCode 
```

### 3.动画效果

```js
$(document).ready(function() {
    // 显示和隐藏
    $("#myDiv").show();
    $("#myDiv").hide();

    // 淡入和淡出
    $("#myDiv").fadeIn();
    $("#myDiv").fadeOut();

    // 滑动效果
    $("#myDiv").slideDown();
    $("#myDiv").slideUp();
});
```



## 三、AJAX请求
```js
$(document).ready(function() {
    $.ajax({
        url: "请求路径",
        method: "请求方式",
        //请求成功执行的函数
        success: function(data) {
            console.log(data);
        },
        //请求失败/异常执行的函数
        error: function(error) {
            console.error(error);
        }
    });
});
```