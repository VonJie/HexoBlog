---
layout: _posts
title: 原生JS的一些操作
date: 2016-05-03 15:02:37
tags: csdn
categories: code
description: 原生JS的一些操作
---

```
window.onload = function(){
  //判断是否选中
  var $cr = $("#cr");		//jquery对象
  var cr = $cr[0];		//DOM对象，或者$cr.get(0);
  $cr.click(function(){
    if (cr.checked) {	//DOM的方式判断
      alert("感谢你的支持！你可以继续操作！JS");
    }
    if($cr.is(":checked")){		//Jquery的方式判断
      alert("感谢你的支持！你可以继续操作！JQ");
    }
  });
  
  
  //是否勾中全选
  $("[name=items]:checkbox").change(function(){
    var flag = true;
    $("[name=items]:checkbox").each(function(){
      if(!this.checked){
        flag = false;
      }
    });
    $("#checkAllNo").attr("checked",flag).prop("checked",flag);
  });


  //选中当前并添加class
  var div = document.getElementsByClassName("jsp")[0];
	var items = div.getElementsByTagName("p");
	for (var i=0;i < items.length;i++) {
		
		items[i].onclick = function(){
			var pls = this.parentNode.childNodes;
			
			for (var i=0;i < pls.length;i++) {
				pls[i].className = "";
			}
			
			this.className = "selected";
		}
	}
	
	
	//隔行换色
	var tb = document.getElementById("tb");		//获取ID为tb的元素（ID是唯一属性，不用加索引[0]）
	var tbody = tb.getElementsByTagName("tbody")[0];
	var tr = tbody.getElementsByTagName("tr");
	for (var i=0;i < tr.length;i++) {
		if(i%2 == 0){			//取模（取余数。例如0%2=0，1%2=1，2%2=0；3%2=1）
			tr[i].style.backgroundColor = "#f00";
		}
	}
	
	
	//判断选中个数
	<div>
    	<input type="checkbox" value="1" name="check" checked="checked" />
    	<input type="checkbox" value="2" name="check" />
    	<input type="checkbox" value="2" name="check" checked="checked" />
    	<input type="button" id="btn" value="你选中的个数" />
    </div>
	var btn = document.getElementById("btn");
	btn.onclick = function(){
		var ary = new Array();		//创建一个数组对象
		var ipt = document.getElementsByName("check");
		
		for (var i=0;i < ipt.length;i++) {
			if (ipt[i].checked) {	//判断是否被选中
				ary.push(ipt[i].value);		//把符合条件的添加到数组当中，push()是javascript中数组的方法
			}
		}
		alert("选中的个数为："+ ary.length)
	}
	
	
	//当前P在所有P元素中的索引值
	<div class="numindex">
		<p>11111111</p>
		<div>11111111</div>
		<p>11111111</p>
		<div>11111111</div>
		<p>11111111</p>
		<div>11111111</div>
	</div>
	//JS方法
	var box = document.getElementsByClassName("numindex")[0];
	var p = box.getElementsByTagName("p");
	for(i = 0 ; i<p.length;i++){
		(function(k){
			p[i].onclick=function(){
				alert(k);
			}
		})(i);
	}
	//JQ方法
	$(".numindex p").each(function(){
		$(this).addClass("a "+$(this).index());
		var arr = $(".a");
		$(this).click(function(){
			var b = $(this).index();
			alert(arr.index($("."+b)));
		});
	});
}
```