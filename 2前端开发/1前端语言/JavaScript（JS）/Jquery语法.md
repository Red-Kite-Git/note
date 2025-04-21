================================================用法===============================================
Jquery是对原生JS的封装，提供了许多快捷的语句与函数

================================================事件函数===============================================
$(document).ready(function(){});     
    //等待页面文档结构加载完成以后执行
    //$(function(){});    （简写）
    // 对比原生JS window.onload事件		等待页面所有内容都加载完成以后再执行

$(元素).focus(function () { });
    //focus()方法 元素获得焦点事件
$(元素).blur(function () { });
    //blur()方法 元素失去焦点事件

$(元素).hover(function () { }, [function () { }]);
    //hover()方法 鼠标移入执行第一个函数，鼠标移出执行第二个函数[没有第二函数则不执行任何操作]
    //对比原生JS（Jquery中也有这两个方法）  mouseover()方法 鼠标悬停事件       mouseout()方法 鼠标移除事件

$(元素).keydown(function () { })
    //keydown方法 键盘按下事件
$(元素).keyup(function () { })
    //.keyup方法 键盘弹起事件
$(元素).keypress(function () { })
    //.keypress方法 键盘按下打印出字符



        

================================================方法函数===============================================

$(元素).css(属性,值);   $(元素).css({属性:值,属性:值});     $(元素).css();
    //css([属性,值])方法 设置元素的样式/获取元素的样式

$(元素).val(文本);        $(元素).val()
    //val()方法 获取或设置元素的值

$(元素).addClass(类名);
    //addClass()方法追加一个或多个类 
$(元素).removeClass(类名);
    //removeClass()方法移除一个或多个类

$(？).toggle(fn1,fn2,fn3);
    //toggle()方法切换显示和隐藏
$(元素).toggleClass(类名);  
    //toggleClass()方法切换类

$(元素).hasClass(类名)
    // 判断是否存在某个类

$(元素).html(标签/内容);     $(元素).html();
    //html()不对HTML代码解析 直接将HTML代码替换到指定元素中或者获取元素中HTML代码
$(元素).text(标签/内容);     $(元素).text();
    //text()对HTML代码进行解析转换为文本 然后替换到指定元素中或则获取元素中文本
$(元素).append(标签/内容);
    //append()方法在被选元素的结尾追加内容

$(元素).append(标签/内容);
    //append()方法在被选元素的子元素结尾插入指定内容
$(元素).prepend(标签/内容);
    //prepend()方法在被选元素的子元素开头插入指定内容
        

$(元素).after(标签/内容);
    //after()方法在被选元素之后插入指定内容
$(元素).before(标签/内容);
    //before()方法在被选元素之前插入指定内容
        
$(元素).remove();
    //remove()方法从被选元素中移除全部元素（包括子元素）
$(元素).empty();
    //empty()方法从被选元素中移除所有子元素

$(元素).replaceWith(标签/内容);
    //replaceWith()方法用指定的内容替换被选元素
$(元素).replaceAll(标签/内容);
    //replaceAll()方法用被选元素替换指定的元素

$(元素).clone(true);
    //clone()方法复制被选元素，包括所有后代元素和文本内容
        
$(元素).attr(属性:值);      $(元素).attr({属性:值,属性:值});     $(元素).attr();
    //attr()方法设置或返回被选元素的属性和值
$(元素).removeAttr("属性");
    //removeAttr()方法从被选元素移除属性

$(元素).index()
    //index()方法 返回指定元素相对于其同级元素的索引位置（从 0 开始计数）

event.keyCode       获取按键的keyCode


================================================选择器===============================================

$(document).ready(function() {
    // ID选择器
    $("#myId").css("color", "red");

    // 类选择器
    $(".myClass").hide();

    // 元素选择器
    $("p").text("Hello, world!");

    // 组合选择器
    $("div, p, .myClass").addClass("selected");

    // 属性选择器
    $("input[name='myName']").val("New Value");
});

================================================动画效果===============================================

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

================================================AJAX请求===============================================

$(document).ready(function() {
    $.ajax({
        url: "https://api.example.com/data",
        method: "GET",
        success: function(data) {
            console.log(data);
        },
        error: function(error) {
            console.error(error);
        }
    });
});

