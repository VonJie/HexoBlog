---
layout: _posts
title: jQuery实用的代码片段
date: 2016-05-27 10:45:09
tags: csdn
categories: 技术
---

```
//1.页面加载完之前执行，与嵌入的JS加载方式一样
(function($){
    alert("start");    
})(jQuery);
//2.页面加载完后执行
$(document).ready(function(){
    alert("jquery ready");
});
//3.页面加载完后执行
$(function(){
    alert("jquery onload");
});
//4.页面加载完后执行
jQuery(function($){
    alert("jquery ready");
});

//修改jQuery默认编码
$.ajaxSetup({
    ajaxSettings:{contentType:"application/x-www/form-urlencoded;charset=GB2312"}
});

//检查各种浏览器
//1.检查Safari：
(if($.browser.safari))
//2.检查IE6及之后版本：
(if($.browser.msie && $.browser.version>6))
//3.检查IE6及之前版本：
(if($.browser.msie && $.browser.version<=6))
//4.检查FireFox2及之后版本：
(if($.browser.mozilla && $.browser.version>='1.8'))

//选中页面上的所有复选框
var tog=false;
$("a").click(function(){
    $("input[type=checkbox]").attr("checked",!tog);
    tog=!tog;
});

//返回顶部代码
$(document).ready(function(){
    jQuery.scrollTo=function(scrollDom,scrollTime){
        $(scrollDom).click(function(){
            $('html,body').animate(
                {
                    scrollTop:$(scrollDom).offset().top
                },
                scrollTime
            );
            return false;
        });
    };
    $.scrollTo("a",1000);
})

//iframe高度自适应
$(document).ready(function(){
    $("iframe").load(function(){
        var vHeight=$(this).contents().find("body").height() + 32;
        $(this).height(vHeight<300?300:vHeight);
    });
});
<iframe src="1.html" frameborder="0" scrolling="no"></iframe>

//左右DIV自适应相同高度
function $(id){
    return document.getElementById(id);
}
function autoHeight(){
    if($("left").offsetHeight >= $("right").offsetHeight){
        $("right").style.height=$("left").style.height + "px";
    }else{
        $("left").style.height=$("right").style.height + "px";
    }
}
window.onload=function(){
    autoHeight();
}

//获取鼠标的位置
$(document).ready(function(){
    $(document).mouse(function(e){
        getScreenCoordinates(e);    //调用函数
    });
});
function getScreenCoordinates(e){
    //在屏幕中
    x=e.screenX;
    y=e.screenY;
    //在窗口客户区
    x=e.clientX;
    y=e.clientY;
    //在窗口页面中
    x=e.pageX;
    y=e.pageY;
    
    $("span").text("X:"+x+",Y:"+y+);
}
```