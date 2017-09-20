---
title: JavaScript String属性与方法  
categories: JavaScript	
tags: JavaScript
---
## String  
在JavaScript中String是一个全局对象，用来构造字符串。  

### 方法  

	String.fromCharCode(num1, ..., numN) 

	String.fromCharCode(65,66,67) //'ABC'

### String实例方法  
String.prototype.charAt()  
　　charAt() 方法返回字符串中指定位置的字符。
	
	str.charAt(index)

	'cat'.charAt(1); // "a"
	'cat'[1]; // "a"  

String.prototype.charCodeAt()  
　　charCodeAt() 方法返回0到65535之间的整数 

	str.charCodeAt(index)

	"ABC".charCodeAt(0) // 65
 
String.prototype.concat()  
> 强烈建议使用 赋值操作符（+, +=）代替 concat 方法。  

　　concat() 方法将一个或多个字符串与原字符串连接合并，形成一个新的字符串并返回。  

	str.concat(string2, string3[, ..., stringN])

	var hello = "Hello, ";
	console.log(hello.concat("Kevin", " have a nice day.")); /* Hello, Kevin have a nice day. */

String.prototype.indexOf()  
　　indexOf() 方法返回指定值在字符串对象中首次出现的位置。从 fromIndex 位置开始查找，如果不存在，则返回 -1。

	str.indexOf(searchValue[, fromIndex])

	"Blue Whale".indexOf("Blue");     // returns  0
	"Blue Whale".indexOf("Blute");    // returns -1
	"Blue Whale".indexOf("Whale", 0); // returns  5

String.prototype.lastIndexOf()  
　　lastIndexOf() 方法返回指定值在调用该方法的字符串中最后出现的位置，如果没找到则返回 -1。从该字符串的后面向前查找，从 fromIndex 处开始。

	str.lastIndexOf(searchValue[, fromIndex])

	"canal".lastIndexOf("a")   // returns 3
	"canal".lastIndexOf("a",2) // returns 1
	"canal".lastIndexOf("a",0) // returns -1
	"canal".lastIndexOf("x")   // returns -1

String.prototype.match()  
　　当字符串匹配到正则表达式（regular expression）时，match() 方法会提取匹配项。返回一个数组 

	str.match(regexp);

String.prototype.replace()  
　　replace() 方法使用一个替换值（replacement）替换掉一个匹配模式（pattern）在原字符串中某些或所有的匹配项，并返回替换后的字符串。这个替换模式可以是字符串或者RegExp（正则表达式），替换值可以是一个字符串或者一个函数。
	
	var re = /apples/gi;
	var str = "Apples are round, and apples are juicy.";
	var newstr = str.replace(re, "oranges");  //"oranges are round, and oranges are juicy."

String.prototype.slice()
　　slice()方法提取字符串中的一部分，并返回这个新的字符串。

	str.slice(beginSlice[, endSlice])

	var str1 = 'The morning is upon us.';
	var str2 = str1.slice(4, -2);
	
	console.log(str2); // OUTPUT: morning is upon u

String.prototype.split()
　　split() 方法通过把字符串分割成子字符串来把一个 String 对象分割成一个字符串数组。

	str.split([separator][, limit])
	
	var myString = "Hello World. How are you doing?";
	var splits1 = myString.split(" ", 3); //["Hello", "World.", "How"]
	var splits2 = myString.split("", 3);  //["H", "e", "l"]

String.prototype.substr()
　　substr() 方法返回字符串中从指定位置开始到指定长度的子字符串。

	str.substr(start[, length])

	var str = "abcdefghij";
	console.log("(1,2): "    + str.substr(1,2));   // (1,2): bc
	console.log("(-3,2): "   + str.substr(-3,2));  // (-3,2): hi
	console.log("(-3): "     + str.substr(-3));    // (-3): hij
	console.log("(1): "      + str.substr(1));     // (1): bcdefghij
	console.log("(-20, 2): " + str.substr(-20,2)); // (-20, 2): ab
	console.log("(20, 2): "  + str.substr(20,2));  // (20, 2):

String.prototype.substring()
　　substring() 返回字符串两个索引之间（或到字符串末尾）的子串。
> 如果 indexStart 大于 indexEnd，则 substring 的执行效果就像两个参数调换了一样。例如，str.substring(1, 0) == str.substring(0, 1)。

	str.substring(indexStart[, indexEnd])

	var anyString = "Mozilla";

	// 输出 "Moz"
	console.log(anyString.substring(0,3));
	console.log(anyString.substring(3,0));
	
	// 输出 "lla"
	console.log(anyString.substring(4,7));
	console.log(anyString.substring(7,4));
	
	// 输出 "Mozill"
	console.log(anyString.substring(0,6));
	
	// 输出 "Mozilla"
	console.log(anyString.substring(0,7));
	console.log(anyString.substring(0,10))

String.prototype.trim()
　　trim() 方法会删除一个字符串两端的空白字符。在这个字符串里的空格包括所有的空格字符 (space, tab, no-break space 等)以及所有的行结束符（如 LF，CR）。
> trim() 方法并不影响原字符串本身，它返回的是一个新的字符串。

	str.trim()

	
	var orig = '   foo  ';
	console.log(orig.trim()); // 'foo'
	
	// 另一个.trim()例子，只从一边删除
	
	var orig = 'foo    ';
	console.log(orig.trim()); // 'foo'

