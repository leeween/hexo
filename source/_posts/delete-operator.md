---
title: 一道题目看delete操作符
categories: 碎片知识
tags: [JavaScript]
---
一道题目：

	(function(x){
		delete x;
		return x;
	})(1);

由于delete操作符没有起作用，最后返回的结果是1。   
因为平常很少用到delete这个属性，所以这里写下加深一下印象。


> delete运算符可以用来删除对象的属性。如果对象包含该属性，那么该属性就会被移除。他不会触及原型链中的任何对象。

值得注意的是：

	x = 1;  // 隐式声明的全局变量
	var y = 2;  // 显式声明的全局变量
	
	delete x;  // true
	delete y;  // false

可以使用delete操作符来删除一个隐式声明的全局变量,也就是没有使用 var 定义的全局变量.隐式声明的全局变量其实是global对象(window)的属性。

不能删除从对象从原型所继承过来的属性
