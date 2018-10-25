---
layout: _posts
title: 有趣的题目
date: 2016-10-11 14:05:07
tags: csdn
categories: code
description: 有趣的题目
---

### 1.鸡兔同笼,共头35,脚96,求兔鸡各多少？
```
for(var rabit=0;rabit<25;rabit++){
    if(rabit*4 + (35-rabit)*2 == 96){
        console.log('兔子:'+rabit+',鸡:'+(35-rabit));
    }
}

function tuotui(x,y){
   return (y-x*2)/2;
}
tuotui(35,96);
```

### 2.一个篮子里有很多很多鸡蛋,2个2个拿多一个,3个3个拿多一个,4个4个拿多一个,5个5个拿多一个,6个6个拿多一个,7个7个刚刚好,篮子里有多少个鸡蛋？
```
for(var i=0;i<1000000;i++){
    if(i%2==1 && i%3==1 && i%4==1 && i%5==1 && i%6==1 && i%7==0){
        console.log(i);
         break;
    }
}
```

### 3.写一个函数，传入一个整数，判断是否为3的N次方，比如1,3,9,27...等返回true,否则返回false.
```
//我的方法
function a(b){
	var x=[],z=false;
    for(var i=0;i<b;i++){
    	var y=(Math.pow(3,i)==b)?true:false;
    	x.push(y);
    }
    for(i in x){
    	if(x[i]){
    		z=x[i];
    		break;
    	}
    }
console.log(x);
console.log(z);
}
//小寒的方法
function a(b){
    for(var i=1;i<=b;i++){
        if(Math.pow(3,i)==b){
            return true;
        }
    }
  return false;
}
//网上的方法
function isPowerOfThree(n){
	if(n<0){
		return false;
	}
	if(n==1){
		return true;
	}
	//Math.log()方法可返回一个数的自然对数
	var index = Math.log(n)/Math.log(3);
	var cur = parseInt(index);//parseInt() 函数可解析一个字符串，并返回一个整数。
	var next = Math.ceil(index);//ceil() 方法可对一个数进行上舍入。
	if(n%3 == 0){
		var temp = Math.pow(3,next) / n;
		if(parseInt(temp) == temp){
			return true;
		}
	}
	return false;
}
```

### 4.给 array 对象添加一个原型方法，传入一个值，判断该数组中是否存在该值
```
Array.prototype.hasvalue=function(vue){ 
	var returnvue=-1;  
	for(var i=0;i<this.length;i++){ 
		if(vue==this[i]){ return i; }  
	}  
	return returnvue;  
}
```

### 5.用两种方法实现打印 li 的索引(必须有闭包的方法)
```
window.onload = function () {
    var aLi = document.getElementsByTagName("li");
    for (var i = 0; i < aLi.length; i++) {
        (function (i) {
            aLi[i].onclick = function () {
                alert(i);
            };
        })(i);
    }
}
```

### 6.js 1234 实现 4321
```
var str="1234";
str.split("").reverse().join("");
```