---
title: javascript_this
date: 2016-04-25 17:23:36
tags: javascript
desc: javascript中this代表什么
---
1. 方法调用模式
在一个对象的函数中，this表示该对象：

	var A = {
		name: 'A_name',
		getName: function(){
			return this.name;
		}
	}
	A.getName();//this=A
	

<!-- more -->

2. 函数调用模式
当函数不是对象的函数时，函数中的this表示全局变量：

	function f(){
		this.name = 'global';
	}
	f();//this=window
	
3. 构造器调用模式
使用new调用的函数，其中的this指向的是被构造的对象：

	function f(){
		this.name = 'className';
	}
	var ff = new f();//this = ff
	
4. 使用apply或call调用模式
函数中的this会绑定到apply或call方法中的第一个参数：

	function getName(name){
		this.name = 'className';
	}
	var f = {};
	getName.call(f,name);//this = f
	
	