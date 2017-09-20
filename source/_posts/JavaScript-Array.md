---
title: JavaScript Array属性与方法  
categories: JavaScript	
tags: JavaScript
---
## Array  
在JavaScript中Array是一个全局对象，用来构造数组。  

### 属性  
Array.length  
　　Array构造函数的length属性，其值为1。 

Array.prototype  
　　允许为所有数组对象附加属性。

### 数组实例方法  
>下面的这些方法会改变调用它们的对象自身的值：  

##### 增删
Array.prototype.pop()  
　　删除数组的最后一个元素，并返回这个元素。  

Array.prototype.push()  
　　在数组末尾增加一个或多个元素，并返回数组的新长度。 

Array.prototype.shift()  
　　删除数组的第一个元素，并返回这个元素。  

Array.prototype.unshift()  
　　在数组开头增加应该IE或多个元素，并返回数组的新长度。  

##### 换序  
Array.prototype.reverse()  
　　颠倒数组中元素的排列顺序，即从最后一个开始排序。  

Array.prototype.sort()  
　　对数组元素进行排序，并返回当前数组。  
　　如果不指定按照哪种顺序进行排列，则按照转换为的字符串的诸个字符的Unicode位点进行排序。   


>下面的这些方法不会改变调用它们的对象的值，只会返回一个新的数组或者返回一个其它的期望值。  

##### 组合
Array.prototype.concat()  
　　返回一个由当前数组和其它若干个数组或者若干个非数组值组合而成的新数组。  

	var arr =[1, 2, 3];
	var newarr = [1, 4, 5];
	arr = arr.concat(newarr, 6);
	// arr = [1, 2, 3, 1, 4, 5, 6]

Array.prototype.slice()  
　　抽取当前数组中的一段元素组合成一个新数组，即浅复制。
	
	var arr = [1, 2, 3];
	arr.slice(0, 1);
	// arr = [1]   

Array.prototype.splice()  
　　在任意的位置给数组添加或删除任意个元素。  
	
	var arr = [1, 2, 3, 4];
	var newarr = arr.splice(1, 2, 3, 4);
	//  newarr = [2, 3]
	//  arr = [1, 3, 4, 4]
	//  从arr[1]开始删除两个，newarr为被删除的数组，在arr[1]添加3和4 

##### 转换为字符串
Array.prototype.join()  
　　连接所有数组元素组成一个字符串。  

	var a = ['Wind', 'Rain', 'Fire'];
	var myVar1 = a.join();      // myVar1的值变为"Wind,Rain,Fire"
	var myVar2 = a.join(', ');  // myVar2的值变为"Wind, Rain, Fire"
	var myVar3 = a.join(' + '); // myVar3的值变为"Wind + Rain + Fire"
	var myVar4 = a.join('');    // myVar4的值变为"WindRainFire"

Array.prototype.toString()  
　　返回一个由所有数组元素组合而成的字符串。  

	var arr = [1, 2, 3];
	var newarr = arr.toString();
	// newarr = '1,2,3'  

Array.prototype.toLocaleString()  
　　返回一个由所有数组元素组合而成的本地化后的字符串。   

##### 索引   
Array.prototype.indexOf()  
　　返回数组中第一个与指定值相等的元素的索引，如果找不到这样的元素，则返回 -1。  

	var array = [2, 5, 9];
	array.indexOf(2);     // 0
	array.indexOf(7);     // -1
	array.indexOf(9, 2);  // 2
	array.indexOf(2, -1); // -1
	array.indexOf(2, -3); // 0

Array.prototype.lastIndexOf()  
　　返回数组中最后一个（从右边数第一个）与指定值相等的元素的索引，如果找不到这样的元素，则返回 -1。
	
	var array = [2, 5, 9, 2];
	var index = array.lastIndexOf(2);
	// index is 3
	index = array.lastIndexOf(7);
	// index is -1
	index = array.lastIndexOf(2, 3);
	// index is 3
	index = array.lastIndexOf(2, 2);
	// index is 0
	index = array.lastIndexOf(2, -2);
	// index is 0
	index = array.lastIndexOf(2, -1);
	// index is 3
	
##### 遍历方法  

Array.prototype.forEach()    
　　为数组中的每个元素执行一次回调函数。  

	var arr = [1, 2, 3];
	arr.forEach(function(element, index, array){
		console.log("a[" + index + "] = " + element*2);	
	});
	// a[0] = 2
	// a[1] = 4
	// a[2] = 6

Array.prototype.every()  
　　如果数组中的每个元素都满足测试函数，则返回 true，否则返回 false。  
	
	var arr = [1, 2, 3];
	arr.every(function(element, index, array){
		return (element < 5);
	});
	// true

Array.prototype.some()  
　　如果数组中至少有一个元素满足测试函数，则返回 true，否则返回 false。  

	var arr = [1, 2, 3];
	arr.some(function(element, index, array){
		return (element > 2);
	});	
	// true

Array.prototype.filter()  
　　将所有在过滤函数中返回 true 的数组元素放进一个新数组中并返回。  

	var arr = [1, 2, 3];
	arr.filter(function(element, index, array){
		return element > 2;
	});	
	// [3]

Array.prototype.map()  
　　返回一个由回调函数的返回值组成的新数组。  

	var numbers = [1, 4, 9];
	var roots = numbers.map(Math.sqrt);
	/* roots的值为[1, 2, 3], numbers的值仍为[1, 4, 9] */

Array.prototype.reduce()  
　　从左到右为每个数组元素执行一次回调函数，并把上次回调函数的返回值放在一个暂存器中传给下次回调函数，并返回最后一次回调函数的返回值。   

	var total = [0, 1, 2, 3].reduce(function(a, b) {
    	return a + b;
	});
	// total == 6 , 0+1+2+3

	var flattened = [[0, 1], [2, 3], [4, 5]].reduce(function(a, b) {
    	return a.concat(b);
	});
	// flattened is [0, 1, 2, 3, 4, 5]

Array.prototype.reduceRight()  
　　从右到左为每个数组元素执行一次回调函数，并把上次回调函数的返回值放在一个暂存器中传给下次回调函数，并返回最后一次回调函数的返回值。  

	var total = [0, 1, 2, 3].reduceRight(function(a, b) {
	    return a + b;
	});
	// total == 6 , 3+2+1+0

	var flattened = [[0, 1], [2, 3], [4, 5]].reduceRight(function(a, b) {
	    return a.concat(b);
	}, []);
	// flattened is [4, 5, 2, 3, 0, 1]

 