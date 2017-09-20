---
title: Freecodecamp——Basic Algorithm Scripting总结
categories: JavaScript
tags: freecodecamp
---
### string方法
---
`string.charCodeAt(pos)`返回string中的pos位置处的字符的字符码位。  

	var str = "JonSnow";  
	var initial = str.charCodeAt(0);  //initial=74  
`String.fromCharCode(char...)`根据一串数字编码返回一个字符串。  

	var a = String.fromCharCode(67, 97, 116);//a是"Cat"  
`string.split(separator, limit)`分割成片段来返回一个字符串数组，limit为可选参数可以限制分割片段数量，separator参数可以使字符串或正则表达式。  

	var str = "Tyrion";  
	var arr1 = str.split(""); // ["T", "y", "r", "i", "o", "n"]
	var arr2 = str.split(" "); // ["Tyrion"]  
    var arr3 = str.split("", 2); // ["T", "y"]  
`string.replace(searchValue, replaceValue)`对`string`进行查找和替换操作，并返回一个新的操作符，参数searchValue可以是一个字符串和正则表达式对象，如果是一个字符串，那么replaceValue只会在第一次出现的地方进行替换，如果是一个正则表达式并且带有g标示则会替换所有，如果没有带g标示，这只会替换第一个。
	
	var str = "A man, a plan, a canal. Panama";
	var result = str.replace(".", "-"); //"ab-cd.ef"
`string.slice(start, end)`返回一个新的字符串，参数为前闭后开，如果start为负数，则与`string.length`相加，end参数可选，默认值为`string.length`。

	var str = "Turn into something beautiful";
	var a = str.slice(0, 4); //"Turn"
	var b = str.slice(20); //"beautiful"
	var c = str.slice(-15, 20); //"thing"
### array方法  
---
`array.reverse()`反转数组内容的顺序。  

	var a = ["1", "2", "3"];
	var b = a.reverse(); //["3", "2", "1"]
`arry.join(separator)`把array构造成一个字符串。首先把array中的每个元素构造成一个字符串，接着用一个separator分隔符把他们连在一起。  

	var a = ["1", "2", "3"];
	var c = a.join(""); //"123"
	var d = a.join("-"); //1-2-3
`array.slice(start, end)`对array中的一段做浅复制，首先复制array[start],一直复制到array[end]为止。end参数可选，默认值为array.length。
	
	var a = ["1", "2", "3"];
	var b = a.slice(0, 1); //["1"]
	var c = a.slice(1); //["2", "3"]
	var d = a.slice(-2); //["2", "3"]
`array.splice(start, deleteCount, item...)`从array中移除一个或多个元素，并用新的item替换它们。参数start是从移除的元素开始，deleteCount是要移除的个数。
	
	var a = ["1", "2", "3"];
	var b = a.splice(1, 1, "4", "5");  //a为["1", "4", "5", "3"]  b为["2"]