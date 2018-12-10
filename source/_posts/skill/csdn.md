---
layout: _posts
title: CSDN 代码迁移
date: 2016-06-03 11:05:47
tags: csdn
categories: 技术
---

### 表单验证
```
$("form :input.required").each(function(){
	var $required = $("<strong class='high'>*</strong>");
	$(this).parent().append($required);
});
$("form :input").blur(function(){
	var $parent = $(this).parent();
	$parent.find(".formtips").remove();
	//验证用户名
	if($(this).is('#username')){
		if(this.value == "" || this.value.length < 6){
			var errorMsg = '请输入至少6位的用户名.';
			$parent.append("<span class='formtips onError'>"+errorMsg+"</span>");
		}else{
			var okMsg = '输入正确';
			$parent.append('<span class="formtips onSuccess">'+okMsg+'</span>');
		}
	}
	//验证邮箱
	if($(this).is('#email')){
		if(this.value=="" || (this.value!="" && !/.+@.+\.[a-zA-Z]{2,4}$/.test(this.value))){
			var errorMsg = '请输入正确的E-Mail地址.';
			$parent.append('<span class="formtips onError">'+errorMsg+'</span>');
		}else{
			var okMsg = '输入正确.';
			$parent.append('<span class="formtips onSuccess">'+okMsg+'</span>');
		}
	}
}).keyup(function(){
	$(this).triggerHandler("blur");
}).focus(function(){
	$(this).triggerHandler("blur");
});		//keyup()和focus()即时判断,triggerHandler获取blur事件，但不执行blur()默认行为（即失去焦点）

$('#refer').click(function(){
	$("form .required:input").trigger('blur');		//模拟失焦事件
	var numError = $('form .onError').length;
	if(numError){	//有值为true,0为false,
		return false;
	}
	alert("注册成功,密码已发送到你的邮箱,请查收.");
});
```
### 翻页动画
```
var page = 1;
var i = 3;
//下一页
$("span.next").click(function(){
	var $parent = $(this).parents("div.v_show");
	var $v_show = $parent.find("div.v_content_list");
	var $v_content = $parent.find("div.v_content");
	
	var v_width = $parent.width();
	var len = $v_show.find("li").length;
	var page_count = Math.ceil(len / i)   //只要不是整数，就往大的方向取最小的整数，如3.1151 ~= 4；
	if (!$v_show.is(":animated")) {
		if (page == page_count) {
			$v_show.animate({left: '0px'},"slow");
			
			page = 1;
		}else{
			$v_show.animate({left: '-='+v_width},"normal");
			
			page++;
		}
		$parent.find("span").eq((page - 1)).addClass("current")
			   .siblings().removeClass("current");
	}
});
//上一页
$("span.prev").click(function(){
	var $parent = $(this).parents("div.v_show");
	var $v_show = $parent.find("div.v_content_list");
	var $v_content = $parent.find("div.v_content");
	
	var v_width = $parent.width();
	var len = $v_show.find("li").length;
	var page_count = Math.ceil(len / i)   //只要不是整数，就往大的方向取最小的整数，如3.1151 ~= 4；
	if (!$v_show.is(":animated")) {
		if (page == 1) {
			$v_show.animate({left: '-='+v_width*(page_count - 1)},"slow");
			
			page = page_count
		}else{
			$v_show.animate({left: '+='+v_width},"normal");
			
			page--;
		}
		$parent.find("span").eq((page - 1)).addClass("current")
			   .siblings().removeClass("current");
	}
});
```
### 鼠标悬停图片预览，制作title提示
```
//图片预览
$(".imgtip li a").mouseover(function(e){
	myTitle = this.title;
	myHref = $(this).find("img").attr("src");
	this.title="";
	var imgtip = "<div id='imgtip'><img src='"+myHref+"'><br />"+myTitle+"</div>";
	
	$("body").append(imgtip);
	var h = $("#imgtip img").height();
	$("#imgtip").css({
			"position": "absolute",
			"top": (e.pageY - h - 20) + "px",
			"left": (e.pageX + 10) + "px"
		}).show("fast");
}).mouseout(function(){
	this.title = myTitle;
	$("#imgtip").remove();
}).mousemove(function(e){
	var h = $("#imgtip img").height();
	$("#imgtip").css({
		"top": (e.pageY - h - 20) + "px",
		"left": (e.pageX + 10) + "px"
	})
});

//title提示
$("a.tooltip").mouseover(function(e){
	this.myTitle = this.title;		//先存起来
	this.title = "";		//再清空
	var tooltip = "<div id='tooltip'>"+this.myTitle +"</div>";
	
	$("body").append(tooltip);
	$("#tooltip")
		.css({
			"position": "absolute",
			"top": (e.pageY + 20) + "px",
			"left": (e.pageX + 10) + "px"
		}).show("fast");
}).mouseout(function(){
	this.title = this.myTitle;
	$("#tooltip").remove();
}).mousemove(function(e){
	$("#tooltip")
		.css({
			"top": (e.pageY + 20) + "px",
			"left": (e.pageX + 10) + "px"
		})
});
```
### Cookie网页更换皮肤
```
var $li = $("#skin li");
	//引用cookie，当用户刷新或关闭网页时，网页依然保持选中的皮肤（jquery.cookie.js）
$li.click(function(){
	switchSkin(this.id)
});
var cookie_skin = $.cookie("MyCssSkin");
if(cookie_skin){	//如果确实存在Cookie
	switchSkin(cookie_skin)
}
function switchSkin(skinName){
	$("#"+skinName).addClass("selected")
		.siblings().removeClass("selected");
	$("#cssfile").attr("href","css/"+skinName+".css");
	$.cookie("MyCssSkin",skinName,{path:'/',expires:10});		//计入cookie； $.cookie(名称,值,{路径,储存时间})
}
```
### 移动端font-size根据屏幕大小改变
```
//当设备的方向变化（设备横向持或纵向持）此事件被触发。绑定此事件时，
//注意现在当浏览器不支持orientationChange事件的时候我们绑定了resize 事件。
//总来的来就是监听当然窗口的变化，一旦有变化就需要重新设置根字体的值
var docEl = document.documentElement,
    resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
    recalc = function() {
        docEl.style.fontSize = 16 * (docE1.clientWidth / 320) + 'px';
    };
//绑定浏览器缩放与加载时间
window.addEventListener(resizeEvt, recalc, false);
document.addEventListener('DOMContentLoaded', recalc, false);

//根据实际情况通过设计稿与当前可视区的大小做一个比例关系，通过这个比例进行缩放处理
//给html设置fontSize大小，其实就是在DOMContentLoaded或者resize变化后调整fontSize的大小，从而调整rem的比值关系。
```
### 事件冒泡
```
//One()方法，为元素绑定处理函数，处理函数触发一次后，立即被删除
$("body").one("click",function(){
	alert(1);
	var txt = $("#msg").html() + "<p>body元素被点击。</p>";
	$("#msg").html(txt);
});

$("#content").one("click",function(){
	alert(2);
	var txt = $("#msg").html() + "<p>content元素被点击。</p>";
	$("#msg").html(txt);
});

$("#content span").bind("click",function(event){	//event:事件对象
	alert(3);
	var txt = $("#msg").html() + "<p>span元素被点击。</p>";
	$("#msg").html(txt);
	//event.stopPropagation();		//停止事件冒泡
	//event.preventDefault()		//阻止默认时间行为，如A标签的跳转，提交按钮等。
	return false;		//等同与上面的效果集合
});
```
### 百度富文本编辑器
```
var ue = UE.getEditor('editor'+j,{
	toolbars: [[
        'fullscreen', 'source', '|', 'undo', 'redo', '|',
        'bold', 'italic', 'underline', 'fontborder', 'strikethrough', 'superscript', 'subscript', 'removeformat', 'formatmatch', 'autotypeset', 'blockquote', 'pasteplain', '|', 'forecolor', 'backcolor', 'insertorderedlist', 'insertunorderedlist', 'selectall', 'cleardoc', '|',
        'rowspacingtop', 'rowspacingbottom', 'lineheight', '|',
        'customstyle', 'paragraph', 'fontfamily', 'fontsize', '|',
        'directionalityltr', 'directionalityrtl', 'indent', '|',
        'justifyleft', 'justifycenter', 'justifyright', 'justifyjustify', '|', 'touppercase', 'tolowercase', '|',
        'link', 'unlink', '|', 'imagenone', 'imageleft', 'imageright', 'imagecenter', '|',
        'simpleupload', 'insertimage', 'emotion', 'scrawl', 'music', 'attachment','|',
        'horizontal', 'date', 'time', 'spechars','|',
        'inserttable', 'deletetable','mergecells', 'mergeright', 'mergedown', 'splittocells', 'splittorows', 'splittocols', 'charts',
	]],
	autoHeightEnabled: true,
	autoFloatEnabled: true,
	initialStyle: 'p{line-height:1em; font-size: 16px; }',
	initialContent: '',
    autoClearinitialContent:true,
    focus: false,
});
ue.addListener("contentChange",function(){
    var s = ue.getContent();
    if ($(s).length == "") {
        var a = "<p>点此编辑『富文本』内容 ——&gt;</p><p>你可以对文字进行<strong>加粗</strong>、<em>斜体</em>、<span style='text-decoration: underline;'>下划线</span>、<span style='text-decoration: line-through;'>删除线</span>、文字<span style='color: rgb(0, 176, 240);'>颜色</span>、<span style='background-color: rgb(255, 192, 0); color: rgb(255, 255, 255);'>背景色</span>、以及字号<span style='font-size: 20px;'>大</span><span style='font-size: 14px;'>小</span>等简单排版操作。</p><p>还可以在这里加入表格了</p><table><tbody><tr><td width='93' valign='top' style='word-break: break-all;'>中奖客户</td><td width='93' valign='top' style='word-break: break-all;'>发放奖品</td><td width='93' valign='top' style='word-break: break-all;'>备注</td></tr><tr><td width='93' valign='top' style='word-break: break-all;'>猪猪</td><td width='93' valign='top' style='word-break: break-all;'>内测码</td><td width='93' valign='top' style='word-break: break-all;'><em><span style='color: rgb(255, 0, 0);'>已经发放</span></em></td></tr><tr><td width='93' valign='top' style='word-break: break-all;'>大麦</td><td width='93' valign='top' style='word-break: break-all;'>积分</td><td width='93' valign='top' style='word-break: break-all;'><a href='javascript: void(0);' target='_blank'>领取地址</a></td></tr></tbody></table><p style='text-align: left;'><span style='text-align: left;'>也可在这里插入图片、并对图片加上超级链接，方便用户点击。</span></p>"
        $(".wb_txt").html(a);
    } else {
        $(".wb_txt").html(s);
    }
});
```
### 确保jQuery库与其它库不发生冲突
```
(function($){				//定义匿名函数并设置形参为$
	$(function(){			//匿名函数内部的$均为jQuery
		$("p").click(function(){	//继续使用$()方法
			alert($(this).text());
		});
	});
})(jQuery);					//执行匿名函数且传递实参jQuery

$("pp").style.display = 'none';		//使用其它js库
```
### 三元运算符
```
$('.tb2 > tbody > tr').click(function(){
	//正常思路
	if($(this).hasClass('selected')){
		$(this)
			.removeClass('selected')
			.find(':checkbox').attr('checked','checked').prop('checked',false);
	}else{
		$(this)
			.addClass('selected')
			.find(':checkbox').attr('checked','').prop('checked',true);
	}
	//三元运算符
	var hasSelected = $(this).hasClass('selected');
	$(this)
		[hasSelected?"removeClass":"addClass"]('selected')	//三元运算符
		.find(':checkbox').attr('checked',!hasSelected).prop('checked',!hasSelected);
});
```
### 简单的轮播图原理
```
var silde = {
	init:function(){
		this.auto();
	},
	auto:function(){
		var _root = this,
			$ul = $("#sidle").find("ul"),
			$lis = $ul.children("li"),
			width = $lis.eq(0).width();
		setInterval(function(){
			$ul.animate({
				'margin-left':'-' + width + 'px'
			},
			'slow',
			function(){
				//此处保证轮播循环
				//将已经轮播过的节点的第一张图片添加到末尾的位置
				//再将margin-left重置为0px;
				//这样就能一直的循环下去。
				$ul.css({'margin-left':0})
					.children('li')
					.last()
					.after(
						$ul.children('li').first()
					);
			});
		},2000
		);
	}
};
silde.init();
```
### JS设置cookie，删除cookie
```
//页面一
<html>
<head>
<title>ex7</title>
<style type="text/css">
* {margin:0}
body {text-align:center;min-width:760px}
div {padding:3px 3px 3px 3px}
#main {width:720px;margin:0 auto;text-align:left;margin-top:30px}
#main div span {width:50px}
</style>

<script type="text/javascript">
/***
* @param {string} cookieName Cookie名称
* @param {string} cookieValue Cookie值
* @param {number} nDays Cookie过期天数
*/
function SetCookie(cookieName,cookieValue,nDays) {
    /*当前日期*/
    var today = new Date();
    /*Cookie过期时间*/
    var expire = new Date();
    /*如果未设置nDays参数或者nDays为0，取默认值1*/
    if(nDays == null || nDays == 0) nDays = 1;
    /*计算Cookie过期时间*/
    expire.setTime(today.getTime() + 3600000 * 24 * nDays);
    /*设置Cookie值*/
    document.cookie = cookieName + "=" + escape(cookieValue)
        + ";expires=" + expire.toGMTString();
}
function login() {
    var username = $("user").value;
    var password = $("pass").value;
    /*是否选中7天内无需登录*/
    var save = $("save").checked;
    if(username=="sww" && password=="sww") {
        if(save) SetCookie("username",username,7);
        else SetCookie("username",username,1);
        /*跳转到ex8.html页面*/
        document.location = "ex8.html";
    } else {
        alert("用户名或密码错误！");
    }
}
function $(id) {
    return document.getElementById(id);
}
</script>
</head>
<body>
    <div id="main">
        <div><span>用户名：</span><input type="text" id="user" /></div>
        <div><span>密码：</span><input type="password" id="pass" /></div>
        <div>
            <input type="checkbox" id="save" />
            7天内无需登录
            <input type="button" onclick="login()" value="登录" />
        </div>
    </div>
</body>
</html>

//页面二
代码

<html>
<head>
<title>ex8</title>
<script type="text/javascript">
/***
*读取指定的Cookie值
*@param {string} cookieName Cookie名称
*/
function ReadCookie(cookieName) {
    var theCookie = "" + document.cookie;
    var ind = theCookie.indexOf(cookieName);
    if(ind==-1 || cookieName=="") return "";
    var ind1 = theCookie.indexOf(';',ind);
    if(ind1==-1) ind1 = theCookie.length;
    /*读取Cookie值*/
    return unescape(theCookie.substring(ind+cookieName.length+1,ind1));
}

function $(id) {
    return document.getElementById(id);
}

function init() {
    var username = ReadCookie("username");
    if(username && username.length>0) {
        $("msg").innerHTML = "<h1>欢迎光临," + username + "!</h1>";
    } else {
        $("msg").innerHTML = "<a href='ex7.html'>请登录</a>";
    }
}
</script>
</head>
<body onload="init()">
    <div id="msg"></div>
</body>
</html>
```
### 仿猎聘网JQ代码片段
```
//三元运算符
$(".change-city").on("click",function(){
	$(this).hasClass("active")?($(this).removeClass("active"),$("#header-index .header-city-select").animate({top:0},200)):($(this).addClass("active"),$("#header-index .header-city-select").animate({top:"50px"},200))
});

//鼠标移入移出事件,仿hover
$(".drop-menu-group").mouseenter(function(){
}).mouseleave(function(){
})

//单个按钮，全选/取消全选
var $input = $(".item-list input[type=checkbox]");
var ary = new Array();
$input.change(function(){
	if($(this).is(":checked")){
		$(this).attr("i","0");
	}else{
		$(this).removeAttr("i");
	}
	
	$input.each(function(){
		if($("[i=0]").length >= 3){
			if($(this).attr("i") != 0){
				$(this).attr("disabled",true);
			}
		}else{
			$input.each(function(){
				if($(this).attr("i") != 0){
					$(this).removeAttr("disabled");
				}
			})
		}
	});
});

//循环DIV
function loop(a,b,c){
	for(var i=0;i<c;i++){
		$(a).append($(b).eq(0).clone(true))
	}
}
```
### （字体/高度）增大变小
```
var $comment = $("#comment");

$(".biggerHeight").click(function(){
	if(!$comment.is(":animated")){
		if($comment.height() < 500){
			$comment.animate({height : "+=50"},400);
		}
	}
});
$(".smallerHeight").click(function(){
	if(!$comment.is(":animated")){
		if($comment.height() > 50){
			$comment.animate({height : "-=50"},400);
		}
	}
});


//字体放大缩小
$(".msg_caption span").click(function(){
	var thisEle = $("#para").css("font-size");	//获取字体大小
	var textFontSize = parseInt(thisEle,10)		//把获取的12px转换成数字类型12，10进制
	var unit = thisEle.slice(-2);				//获取单位，px,em均为后两位
	var cName = $(this).attr("class");
	
	if(cName == "biggerSize"){
		if(textFontSize < 22){
			textFontSize +=2;
		}
	}else if(cName == "smallerSize"){
		if(textFontSize > 12){
			textFontSize -=2;
		}
	}
	
	$("#para").css("font-size",textFontSize+unit);
});
```
### 根据滚动条高度隐藏/显示（返回顶部）
```
//按钮绑定事件
$('#backTop').on('click',go);

//滚动条事件
$(window).on('scroll',function(){
	checkPosition($(window).height()-200);
});

//返回顶部
function go(){
	$('html,body').animate({scrollTop:0},800);
}

//检查滚动条位置
function checkPosition(pos){
	if($(window).scrollTop() > pos){
		$('#backTop').fadeIn();
	}else{
		$('#backTop').fadeOut();
	}
}

//初始化检查
checkPosition($(window).height()-100);
```
### 向未来的元素添加事件处理程序
```
// delegate() 方法为指定的元素（属于被选元素的子元素）添加一个或多个事件处理程序，并规定当这些事件发生时运行的函数。
// 使用
// delegate() 方法的事件处理程序适用于当前或未来的元素（比如由脚本创建的新元素）。

$(document).ready(function(){
  $("div").delegate("p","click",function(){
    $(this).slideToggle();
  });
  $("button").click(function(){
    $("<p>这是一个新段落。</p>").insertAfter("button");
  });
});

// live() 方法为被选元素附加一个或多个事件处理程序，并规定当这些事件发生时运行的函数。

// 通过 live() 方法附加的事件处理程序适用于匹配选择器的当前及未来的元素（比如由脚本创建的新元素）。
$(document).ready(function(){
  $("p").live("click",function(){
    $(this).slideToggle();
  });
  $("button").click(function(){
    $("<p>This is a new paragraph.</p>").insertAfter("button");
  });
});
```
### 文字闪烁效果
```
$(document).ready(function(){
    function textFlash(obj,cssName,times){
    	var i=0,
    		t=false,
    		o=obj.attr("class"),
    		c="",
    		times=times||2;			//如果没有设置闪烁次数，就默认为2次
    	if(t) return;
    	t=setInterval(function(){  //实现闪烁功能，通过setInterval()方法不停执行函数
    		i++;					//i不断累加
    		c=i%2?o+cssName:o;		//通过（i%2）切换class
    		obj.attr("class",c);	//设置class来改变文本颜色
    		if (i==2*times) {		//闪烁过程是一红一黑两个步骤，所以闪烁3次即切换2*3=6次
    			clearInterval(t);	//清楚闪烁功能，即清除setInterval()方法
    			obj.removeClass(cssName);	//恢复最初始颜色
    		};
    	},200);
    };
    $(function(){
    	//自动闪烁文本
    	textFlash($("#id-div-autoFlash"),"red",3);
    	//单机闪动文本
    	$("#id-div-clickFlash").bind({
    		click:function(){
    			textFlash($(this),"red",3);
    			return false;
    		}
    	});
    	//通过 E-mail 格式校验闪动文本
    	$("#id-input-emailFlash").blur(function(){
    		//使用正则表达式校验 E-mail 格式字符串
    		if(/^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*/.test($(this).val())){
    			textFlash($(this),"red",3);
    		}
    	})
    });
});
/*
.cssFlashred{color: red;}

<div id="id-div-autoFlash" class="cssFlash">文本自动闪烁</div>
<div id="id-div-clickFlash" class="cssFlash">单机文本闪烁</div>
<input id="id-input-emailFlash" class="cssFlash" type="email" placeholder="如果输入email地址会闪动" />
*/
```
### jQuery页面加载后居中显示消息框的方法
```
function center(obj){
	$.center=function(obj){
		var screenWidth=$(window).width();	//当前浏览器窗口的宽度
		var screenHeight=$(window).height();	//当前浏览器窗口的高度
		var scrolltop=$(document).scrollTop();	//获取当前窗口距离页面顶部高度
		var objLeft=(screenWidth-obj.width())/2;	
		var objTop=(screenHeight-obj.height())/2+scrolltop;
		obj.css({
			left:objLeft + 'px',
			top:objTop + 'px',
			display:'block'
		});
	}

	$.center(obj);
	
	$(window).resize(function(){
		$.center(obj);
	});
	$(window).scroll(function(){
		$.center(obj);
	})
}
```
### 兼容click和tap事件
```
//兼容click和tap事件, 调用时用$(selector).on(CLICK,fn);
var UA = window.navigator.userAgent;
var CLICK = 'click';
if(/iPad|iPhone|Android/.test(UA)){
    CLICK = 'tap';
}
```
### js防止脚本注入
```
function html_encode(str) {
    var s = "";
    if (str.length == 0) return "";
    s = str.replace(/</g, "&lt;");
    s = s.replace(/>/g, "&gt;");
    s = s.replace(/ /g, "&nbsp;");
    s = s.replace(/\'/g, "&#39;");
    s = s.replace(/\"/g, "&quot;");
    s = s.replace(/\n/g, "<br>");
    return s;
}
function encode_html(str) {
    var s = "";
    if (str.length == 0) return "";
    s = str.replace(/&lt;/g, "<");
    s = s.replace(/&gt;/g, ">");
    s = s.replace(/&nbsp;/g, " ");
    s = s.replace(/&#39;/g, "\'");
    s = s.replace(/&quot;/g, '\"');
    s = s.replace(/<br>/g, "\n");
    return s;
}
$(function () {
    $("input,textarea").each(function () {
        $(this).blur(function () {
            var str = $(this).val();
            $(this).val(html_encode(str));
        }).focus(function () {
            var str = $(this).val();
            $(this).val(encode_html(str));
        });
    });
})
```
### 绑定事件，传递数据
```
$(function(){
    $("#id").bind('click',{d1:'Jquery:',d2:'将事件和函数绑定到元素'},myBindFunc(e));
})
function myBindFunc(e){
    $("#div-log").html($("#div-log").html+"<p>"+e.data.d1+e.data.d2+"</p>");
}
```
### DIY手机端商城主页代码片段（jquery一些操作）
```
//当前元素定位
function pos(e){
	//查询当前模块所处页面位置，设置模块属性位置
	var y = $(e).offset().top;
	var x = $(e).offset().left;
	$(e).css("top",y);
	$(e).css("left",x);
	$("html,body").animate({scrollTop:y},0);
}


//图片上传后图显示
<img id="img_Photo" src="" />
<input type="file" onchange="onChangeFile(this)" />
function onChangeFile(sender){
    var filename = sender.value;
    if (filename == "") {
        return "";
    }
    var ExName = filename.substr(filename.lastIndexOf(".") + 1).toUpperCase();
    if (ExName == "JPG" || ExName == "BMP" || ExName == "GIF" || ExName == "PNG") {
    	//店招图片
     	document.getElementById("img_Photo").src = window.URL.createObjectURL(sender.files[0]);
    }
    else {
        alert('请选择正确的图片格式！');
        sender.value = null;
        return false;
    }
}

//rgb(10进制)转#fff(16进制)的方法
function RGBToHex(rgb) {
    var regexp = /^rgb\(([0-9]{0,3})\,\s([0-9]{0,3})\,\s([0-9]{0,3})\)/g;
    var re = rgb.replace(regexp, "$1 $2 $3").split(" ");//利用正则表达式去掉多余的部分
    var hexColor = "#";
    var hex = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F'];
    for (var i = 0; i < 3; i++) {
        var r = null;
        var c = re[i];
        var hexAr = [];
        while (c > 16) {
            r = c % 16;
            c = (c / 16) >> 0;
            hexAr.push(hex[r]);
        }
        hexAr.push(hex[c]);
        hexColor += hexAr.reverse().join('');
    }
    return hexColor;
}


//URL地址选取（爽爽挝咖）
var string = window.location.href;//获取url地址
var index = string.indexOf("=");//indexOf()返回字符串指定出现的位置
var str = string.substring(index + 1, string.length);//substring()方法用于提取字符串中介于两个指定下标之间的字符。
var num = str.replace(/[^0-9]/ig, ""); //从字符串中取数字
```
### 1元购代码片段
```
var hotQuestion = document.getElementById("hotQuestion");
var list = hotQuestion.getElementsByTagName("li");
for(var i=0;i<list.length;i++){
	list[i].onclick = function(k){
		return function(){
            for(var i=0;i<list.length;i++){
	    		var curLi = list[i];
	    		    div = $api.last(curLi, 'div'),
	    			span = $api.first(curLi, 'span');
	    		if(i==k){
	    			$api.attr(div, 'style', 'display:block');
		    		$api.addCls(span, 'rotation');
	    		}else{
		    		$api.attr(div, 'style', 'display:none');
		    		$api.removeCls(span, 'rotation');
	    		}
	    	}
        };
	}(i);
}
```
### 倒计时和累加时间
```
//倒计时
startTimeOut = function ($this, currMS, type) {
	var m = 0;//分钟
	var s = 0;//秒
	var o = 0;//毫秒
	var i = 0;//计数器
	var v;		//时钟对象

	//var currTime = new Date();
	//var endTime = new Date();
	//endTime.setSeconds(endTime.getSeconds() + currMS/1000 - 1);
    //var w = parseInt((endTime.getTime() - currTime.getTime()) / 1000);
	var w = currMS/1000;
	if (w < 0)
		return;
	m = parseInt(w / 60);
	s = parseInt(w % 60);
	o = (w == 0) ? currMS : 99;
	this.countdown = function () {
		v = setInterval(function() {
		    i++;
		    if (type == "singleLottery" && $("#singleLottery").attr("style").indexOf("top:0")<0) {
		        $("#singleLottery").attr("style", "top:0;position:relative;padding:0;margin:0;");//将滚动条还原到第一行,此处避免滚动中的回零失效问题
		    }
			if (m <= 0 && s <= 0 && o <= 0) {
			    //开始揭晓 spanTime1
			    clearInterval(v);
			    refreshLottery($this,type);
				return;
			}
			o = o - Math.round(Math.random() * 19 + 1);

			if (i >= 5) {
				i = 0;
				//Math.round(Math.random()*9+1);
				s = s - 1;
				/*if (s == 0 && m > 0)
					m = m - 1;
				else if (s < 0 && m > 0)
					s = 59;*/
				if (s <= 0 && m > 0) {
				    s = 59;
				    m = m - 1;
				}
				if (m <= 0 && s <= 0)
					o = 0;
				if (m > 0 || s > 0)
					o = 99;
			}
			if (m <= 0) m = 0;
			if (s <= 0) s = 0;

			var mStr = m < 10 ? ('0' + m) : m;
			var sStr = s < 10 ? ('0' + s) : s;
			var oStr = o < 10 ? ('0' + o) : o;
			$this.html(mStr + ":" + sStr + ":" + oStr);
		}, 200);
		timer.push(v);
	}
}


//时间累加计算方法 2017-01-09 16:45 Jesse
function HaveOrderTime(timeBox,currMS) {
	var d,h,m,s;
	var AS = parseInt(currMS / 1000);
	if(AS > 0){
		if(AS > 59){//秒
			if(parseInt(AS / 60) > 59){//分
				if(parseInt(AS / 3600) > 24){//小时
					s=parseInt(AS % 86400 % 3600 % 60);
					m=parseInt(AS % 86400 % 3600 / 60);
					h=parseInt(AS % 86400 / 3600);
					d=parseInt(AS / 86400);
				}else{
					s=parseInt(AS  % 3600 % 60);
					m=parseInt(AS % 3600 / 60);
					h=parseInt(AS / 3600);
					d=0;
				}
			}else{
				s=parseInt(AS % 60);
				m=parseInt(AS / 60);
				d=h=0;
			}
		}else{
			s = AS;
			d=h=m=0;
		}
	}else{
		return;
	}
	console.log(d+','+h+','+m+','+s);
	setInterval(function() {
	    s++;
	    if(s > 59){
	    	if(m+1>59){
	    		if(h+1>24){
	    			d++;
	    			h=m=s=0;
	    		}else{
	    			h++;
	    			m=s=0;
	    		}
	    	}else{
	    		m++;
	    		s=0;
	    	}
	    }
	    var txt=dStr=hStr=mStr=sStr='';
    	if(s>0)sStr =s+'秒';
    	if(m>0)mStr =m+'分';
    	if(h>0)hStr =h+'小时';
    	if(d>0)dStr =d+'天';
	    
	    timeBox.textContent = dStr+hStr+mStr+sStr;
	}, 1000);
}
```
### 募才网移动端表单验证
```
$("input[type=text]").focus(function () {
    $(this).prev("p").find("span").text("");
})
var flag;
var pat = new RegExp("^[A-Za-z0-9]+$");
function check() {
    flag = true;
    var password = $("#TextBox2").val().trim();
    var comfirm = $("#TextBox3").val().trim();
    checkIt("#TextBox1", '请输入至少 6 位的用户名', '用户名由字母、数字组成');
    checkIt("#TextBox2", '请输入至少 6 位的密码', '密码由字母、数字组成');
    if ($("#txtt").val() == "2") {
        if (comfirm == "") {
            $("#TextBox3").prev("p").find("span").text("确认密码不能为空");
            flag = false;
        } else if (comfirm != password) {
            $("#TextBox3").prev("p").find("span").text("两次密码输入不一致");
            flag = false;
        }
    }
    return flag;
}

function checkIt(id, msg1, msg2) {
    var val = $(id).val().trim();
    if (val == "" || val.length < 6) {
        var errorMsg = msg1;
        flag = false;
    } else if (!pat.test(val)) {
        var errorMsg = msg2;
        flag = false;
    }
    if (flag == false) {
        $(id).prev("p").find("span").text(errorMsg);
    }
}
```