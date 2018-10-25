---
layout: _posts
title: APIClound项目JS代码片段
date: 2016-07-12 10:25:51
tags: csdn
categories: code
description: APIClound项目JS代码片段
---

```
//JSON下面的数组有长度
var json1={"abc":[{"name":"txt1"},{"name","txt2"}]};
for(var i=0;i<json1.abc.length;i++){alert(json1.abc[i].name);}
//json本身没有length属性
var json2={"name":"txt1","name2":"txt2"};

//获取Json的length
function getJsonLength(jsonData){
	var jsonLength = 0;
	for(var item in jsonData){
		jsonLength++;
	}
	return jsonLength;	
}

----------------------------------------------------

//getTime() 方法可返回距 1970 年 1 月 1 日之间的毫秒数。
//Date 对象用于处理日期和时间。
//setHours() 方法用于设置指定的时间的小时字段。
//parseInt() 函数可解析一个字符串，并返回一个整数。
----------------------------------------------------

//营业时间周一至周五,8:30-18:00
var a=new Date();
var b=new Date();
var c=new Date();
var week=c.getDay();
a.setHours(8,30,00);
b.setHours(18,00,00);
if(!(c>a && b>c) || week==0 || week==6){
	c.setTime();
   console.log(c);
}

----------------------------------------------------

//split() 方法用于把一个字符串分割成字符串数组

"2:3:4:5".split(":");
//将返回["2", "3", "4", "5"]

"|a|b|c".split("|")	;
//将返回["", "a", "b", "c"]


//Math.random()
//Math 对象用于执行数学任务。
//random() 方法可返回介于 0 ~ 1 之间的一个随机数。

----------------------------------------------------

//验证码
<input type="text" onclick="createCode()" readonly="readonly" id="checkCode" />
var code; //在全局 定义验证码  
function createCode() {
	code = "";
	var codeLength = 6; //验证码的长度  
	var checkCode = document.getElementById("checkCode");
	var selectChar = new Array(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'); //所有候选组成验证码的字符，当然也可以用中文的  

	for (var i = 0; i < codeLength; i++) {

		var charIndex = Math.floor(Math.random() * 36);
		code += selectChar[charIndex];

	}
	//       alert(code);  
	if (checkCode) {
		checkCode.className = "code";
		checkCode.value = code;
	}

}

//不区分大小写对比（验证码）
String.prototype.compare = function(str){
	if(this.toLowerCase() == str.toLowerCase()){
	   return "1"; // 正确
	}
	else{
	   return "0"; // 错误
	}
}

----------------------------------------------------

//倒计时,短信验证码
<input type="button" id="btn" value="免费获取验证码" />  
<script type="text/javascript">  
var wait=3;  
function time(o) {  
        if (wait == 0) {  
            o.removeAttribute("disabled");            
            o.value="免费获取验证码";  
            wait = 3;  
        } else {  
            o.setAttribute("disabled", true);  
            o.value="重新发送(" + wait + ")";  
            wait--;  
            setTimeout(function() {  
                time(o)  
            },  
            1000)  
        }  
    }  
document.getElementById("btn").onclick=function(){time(this);}  
</script>  

----------------------------------------------------
```