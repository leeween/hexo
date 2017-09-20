---
title: nodejs学习笔记(2)
categories: nodejs
tags: nodejs
---
# [freecodecamp](https://www.freecodecamp.com)之node学习笔记2 
---------- 
### Start a Nodejs Server
> 内容出自[learnyounode](https://github.com/workshopper/learnyounode)并在[c9]( http://c9.io)上运行    

1.**HELLO WORLD**  
在仓库内建立program.js文件，写入`console.log("HELLO NODEJS");`然后运行`node program.js`，会在命令行看到打印的HELLO NODEJS  
2.**BABY STEPS**  
`process`是一个全局对象，拥有一个数组其中包含完整命令行的`argv`属性，在program.js文件中写入`console.log(process.argv)`，然后运行`node program.js 1 2 3`，会在命令行看到打印的

		[
			'node',
			'program.js',
			'1',
			'2',
			'3'
		]
需要思考的是怎样依次通过这些arguments，来计算出他们的和。由于数组中的第一个元素总是'node'，第二个元素总是'program.js'。因此我们需要从第三个元素开始循环（index2），同时需要注意的是这些数据是字符串，我们需要将其转化成数字。

		var sum = 0;
		for(var i=2; i<process.argv.length; i++) {
		    sum += Number(process.argv[i]);
		}
		console.log(sum);
在命令行中输入`node program.js 1 2 3`会得到打印结果6。  
3.**MY FIRST I/O**  
（题外：竟然有language选择，太好了。。。之前还在一个个对着词典查）  
QUESTION:  
　　编写一个程序，执行一个同步的对文件系统的操作，读取一个文件，并且在终端打印出这个文件中的内容的行(\n)数。(所要读取的文件的完整路径会在命令行第一个参数提供)  
TIPS:  
　　要执行一个对文件系统的操作，需要用到 fs 这个 Node核心模块。  
　　要读取一个文件，你将需要使用  fs.readFileSync('file')方法。  
　　Buffer 对象是 Node 用来高效处理数据的方式，无论该数据是ascii还是二进制文件，或者其他的格式。Buffer 可以很容易地通过调用 `toString()`  方法转换为字符串。如：`var str = buf.toStrin`  
program.js  
	
		var fs = require("fs");
		var filename = process.argv[2];
		var str = fs.readFileSync(filename).toString();
		var num = str.split("\n").length-1;
		console.log(num);  
		// 只要把 'utf8' 作为 readFileSync 的第二个参数传入  
	    // 就可以不用 .toString() 来得到一个字符串  
	    //  
	    // fs.readFileSync(process.argv[2], 'utf8').split('\n').length - 1;
在命令行输入`node program.js program.js`会得到打印结果8。   
4.**MY FIRST ASYNC I/O**  
QUESTION:  
　　编写一个程序，执行一个异步的对文件系统的操作，读取一个文件，并且在终端打印出这个文件中的内容的行(\n)数。(所要读取的文件的完整路径会在命令行第一个参数提供)  
TIPS:  
　　和前一个问题一样，但是需要使用更加符合nodejs风格的方式解决：异步  你将要使用`fs.readFile()`方法，而不是`fs.readFileSync()`，并且你需要从你所传入的回调函数中去收集数据（这些数据会作为第二参数传递给你的回调函数），而不是使用方法的返回值。  
　　我们习惯中的 Node.js 回调函数都有像如下所示的特征：  
   
   		function callback (err, data) { 
			/* ... */ 
		}
　　所以，可以通过检查第一个参数的真假值来判断是否有错误发生。如果没有错误发生，你会在第二个参数中获取到一个 Buffer 对象。和 `readFileSync()`一样，你可以传入 'utf8'作为它的第二个参数，然后把回调函数作为第三个参数，这样，你得到的将会是一个字符串，而不是 Buffer。  
program.js:  
	
		var fs = require("fs");
		var filename = process.argv[2];
		fs.readFile(filename, function(err, data){
		    if(err) {
		        throw err;
		    }
			// 你也可以使用 fs.readFile(file, 'utf8', callback)
		    var lines = data.toString().split('\n').length-1;
		    console.log(lines)
		});
在命令行输入`node program.js program.js`会得到打印结果8。  
5.**FILTERED LS**  
QUESTION:  
　　编写一个程序来打印出指定目录下的文件列表，并且以特定的文件名扩展名来过滤这个列表。这次会有两个参数提供给你，第一个是所给的文件目录路径（如：path/to/dir），第二个参数则是需要过滤出来的文件的扩展名。  
　　举个例子：如果第二个参数是 txt，那么你需要过滤出那些扩展名为 .txt的文件。  
　　注意，第二个参数将不会带有开头的 .。  
　　你需要在终端中打印出这个被过滤出来的列表，每一行一个文件。另外，你必须使用异步的 I/O 操作  
TIPS:  
　　fs.readdir()方法接收两个参数：第一个是一个路径，第二个则是回调函数。  
　　node 自带的 path 模块也很有用，特别是它那个 extname 方法。 
		
		path.extname(index.html);
		//return '.html'
		path.extname('index.coffee.md')
		// returns '.md'
		path.extname('index.')
		// returns '.'
		path.extname('index')
		// returns ''
		path.extname('.index')
		// returns ''
program.js:  
	
		var fs = require("fs");
		var path = require("path");
		var content = process.argv[2];
		var ext = process.argv[3];
		fs.readdir(content, function(err, list){
		    if(err) {
		        throw err;
		    }
		    for(var i=0; i<list.length; i++) {
		        var file = list[i];
		        if (path.extname(file) === '.' + ext) {
		            console.log(file);
		        }
		    }
			//list.forEach(function(file) {  
	        //   if (path.extname(file) === '.' + ext) {  
	        //       console.log(file)  
	        //   }  
	        //})
		});
6.**MAKE IT MODULAR**   
QUESTION:  
　　编写一个模块(module)，这个模块必须导出（export）一个函数，这个函数将接收三个参数：目录名、文件扩展名、回调函数，并按此顺序传递。文件扩展名必须和传递给你的程序的扩展名字符串一模一样。  
　　这个回调函数必须以 Node 编程中惯用的约定形式（err,data）去调用。这个约定指明了，除非发生了错误，否则所传进去给回调函数的第一  个参数将会是null，第二个参数才会是你的数据。在本题中，这个数据将会是你过滤出来的文件列表，并且是以数组的形式。如果你接收到了一个错误，如：来自 fs.readdir()的错误，则必须将这个错误作为第一个，也是唯一的参数传递给回调函数，并执行回调函数。  
　　你绝对不能直接在你的模块文件中把结果打印到终端中，你只能在你的原始程序文件中编写打印结果的代码。  
　　当你的程序接收到一些错误的时候，请简单的捕获它们，并且在终端中打印出相关的信息。   
 
　　这里有四则规定，你的模块必须遵守：  
   
　　» 导出一个函数，这个函数能准确接收上述的参数。                                
　　» 当有错误发生，或者有数据的时候，准确调用回调函数。                          
　　» 不要改变其他的任何东西，比如全局变量或者 stdout。                           
　　» 处理所有可能发生的错误，并把它们传递给回调   
TIPS:  
　　通过创建一个仅包含目录读取和文件过滤相关的函数的文件来建立一个新的模块。要使模块导出（export）单一函数（single function），你可以将你的函数赋值给module.exports 对象：  
   
     module.exports = function (args) { /* ... */ }  
   
　　或者你也可以使用命名函数，然后把函数名赋值给 module.exports。  
   
　　要在你原来的程序中使用你新的模块，请用 require() 载入你的模块，这和载入 fs模块时候用 require('fs') 一样，唯一的区别在于你的本地模块需要加上 './'这个相对路径前缀。所以，如果你的模块文件名字是mymodule.js，那么你需要像这样写：  
   
     var mymodule = require('./mymodule.js')  
   
　　'.js' 这个文件扩展名通常是可以省略的。  
   
　　现在，mymodule 这个变量就指向了你的模块中module.exports了，因为你刚导出了一个单一的函数，所以现在所声明的变量mymodule 就是那个模块所导出的函数了，你可以直接调用它了！  
   
　　同样，请记住，尽早捕获你的错误，并且在回调中返回：  
   
    function bar (callback) {  
       foo(function (err, data) {  
         if (err)  
           return callback(err) // 尽早返回错误  
       
         // ... 没有错误，处理 `data`  
       
         // 一切顺利，传递 null 作为 callback 的第一个参数  
       
         callback(null, data)  
       })  
    }  
  
mymodule.js:  
	
	var fs = require("fs");
	var path = require("path");
	module.exports = function(dir, ext, callback) {
	    fs.readdir(dir, function(err, data){
	        if (err) 
	            return callback(err);
	        
	        data = data.filter(function (file) {
	            return path.extname(file) === '.' + ext;
	        })
	        callback(null, data);
	    })
	}

program.js:  

	var mymodule = require("./mymodule");
	var content = process.argv[2];
	var ext = process.argv[3];
	mymodule(content, ext, function (err, data) {  
	    if (err)  
	        return console.error('There was an error:', err)  
	
	    data.forEach(function (file) {  
	        console.log(file)  
	    })  
	}) 
  
7.**HTTP CLIENT**   
QUESTION:  
　　编写一个程序来发起一个 HTTP GET 请求，所请求的 URL为命令行参数的第一个。然后将每一个 "data"事件所得的数据，以字符串形式在终端（标准输出 stdout）的新的一行打印出来。  
TIPS:  
　　http.get() 方法是用来发起简单的 GET请求的快捷方式，使用这个方法可以一定程度简化你的程序。http.get()的第一个参数是你想要 GET 的 URL，第二个参数则是回调函数。  
   
　　与其他的回调函数不同，这个回调函数有如下这些特征：  
   
     function callback (response) { /* ... */ }  
   
　　response 对象是一个 Node 的 Stream 类型的对象，你可以将 Node Stream当做一个会触发一些事件的对象，其中我们通常所需要关心的事件有三个："data"，"error" 以及 "end"。你可以像这样来监听一个事件：  
   
     response.on("data", function (data) { /* ... */ })  
   
　　'data'事件会在每个数据块到达并已经可以对其进行一些处理的时候被触发。数据块的大小将取决于数据源。  
   
　　你从 http.get() 所获得的 response 对象/Stream 还有一个 setEncoding()的方法。如果你调用这个方法，并为其指定参数为 utf8，那么 data事件中会传递字符串，而不是标准的 Node Buffer 对象，这样，你也不用再手动将Buffer 对象转换成字符串了。  
http.js:  

	var http = require("http");
	var url = process.argv[2];
	http.get(url, function(res){
    	res = res.setEncoding();
    	res.on("data", console.log);
    	res.on("error", console.error);
	}).on("error", console.error);

8.**HTTP COLLECT**  
QUESTION:  
　　编写一个程序，发起一个 HTTP GET 请求，请求的 URL为所提供给你的命令行参数的第一个。收集所有服务器所返回的数据（不仅仅包括"data" 事件）然后在终端（标准输出 stdout）用两行打印出来。  
   
　　你所打印的内容，第一行应该是一个整数，用来表示你所收到的字符串内容长度，第二行则是服务器返回给你的完整的字符串结果。  
TIPS:  
　　你可以把一个 stream pipe 到 bl 或 concat-stream中去，它们会为你收集数据。一旦 stream传输结束，一个回调函数会被执行，并且，这个回调函数会带上所收集的数据：  
   
     response.pipe(bl(function (err, data) { /* ... */ }))  
     // 或  
     response.pipe(concatStream(function (data) { /* ... */ }))  
   
　　要注意的是你可能需要使用 data.toString() 来把 Buffer 转换为字符串。  
collection.js：  

	var http = require("http");
	var bl = require("bl");
	var url = process.argv[2];
	
	http.get(url, function(res) {
	    res.pipe(bl(function(err, data){
	        if (err) 
	            return console.error(err);
	        data = data.toString();
	        console.log(data.length);
	        console.log(data);
	    }))
	})

9.**JUGGLING ASYNC**   
QUESTION:  
　　这一次，将有三个 URL 作为前三个命令行参数提供给你。  
   
　　你需要收集每一个 URL 所返回的完整内容，然后将它们在终端（标准输出stdout）打印出来。这次你不需要打印出这些内容的长度，仅仅是内容本身即可（字符串形式）；每个 URL 对应的内容为一行。重点是你必须按照这些 URL在参数列表中的顺序将相应的内容排列打印出来才算完成。  
TIPS:  
　　不要期待这三台服务器能好好的一起玩耍！他们可能不会把完整的响应的结果按照你希望的顺序返回给你，所以你不能天真地只是在收到响应后直接打印出来，因为这样做的话，他们的顺序可能会乱掉。  
   
　　你需要去跟踪到底有多少 URL完整地返回了他们的内容，然后用一个队列存储起来。一旦你拥有了所有的结果，你才可以把它们打印到终端。  
   
　　对回调进行计数是处理 Node中的异步的基础。  
async.js   

	var http = require("http");
	var bl = require("bl");
	var list = [];
	var count = 0;
	
	for (var i=0; i<3; i++) {
	    httpGet(i);
	}
	
	function httpGet (index) {
	    http.get(process.argv[2 + index], function(res) {
	        res.pipe(bl(function(err, data){
	            if (err) 
	                return console.error(err);
	                
	            list[index] = data.toString();
	            count++;
	            
	            if(count === 3) {
	                printList();
	            }
	        }))
	    })
	}
	function printList () {
	    for (var i =0; i<3; i++) {
	        console.log(list[i]);
	    }
	}

10.**TIME SERVER**   
QUESETION:  
　　你的服务器应当监听一个端口，以获取一些 TCP连接，这个端口会经由第一个命令行参数传递给你的程序。针对每一个 TCP连接，你都必须写入当前的日期和24小时制的时间，如下格式：  
   
     "YYYY-MM-DD hh:mm"  
   
　　然后紧接着是一个换行符。  
   
　　月份、日、小时和分钟必须用零填充成为固定的两位数：  
   
     "2013-07-06 17:42"   
TIPS:   
　　这次练习中，我们将会创建一个 TCP 服务器。这里将不会涉及到任何 HTTP的事情，因此我们只需使用 net 这个 Node核心模块就可以了。它包含了所有的基础网络功能。  
   
　　net 模块拥有一个名叫 net.createServer() 的方法，它会接收一个回调函数。和Node 中其他的回调函数不同，createServer()所用的回调函数将会被调用多次。你的服务器每收到一个 TCP连接，都会调用一次这个回调函数。这个回调函数有如下特征：  
   
     function callback (socket) { /* ... */ }  
   
　　net.createServer() 也会返回一个 TCP 服务器的实例，你必须调用server.listen(portNumber) 来让你的服务器开始监听一个特定的端口。  
   
　　一个典型的 Node TCP 服务器将会如下所示：  
   
     var net = require('net')  
     var server = net.createServer(function (socket) {  
       // socket 处理逻辑  
     })  
     server.listen(8000)  
   
　　记住，请一定监听由第一个命令行参数指定的端口。  
 
　　socket 对象包含了很多关于各个连接的信息（meta-data），但是它也同时是一个Node 双工流（duplexStream），所以，它即可以读，也可以写。对这个练习来说，我们只需要对 socket写数据和关闭它就可以了。  
   
　　使用  socket.write(data) 可以写数据到 socket 中，用  socket.end()可以关闭一个 socket。另外， .end()方法也可以接收一个数据对象作为参数，因此，你可简单地使用 socket.end(data)来完成写数据和关闭两个操作。    
service.js   

	var net = require("net");

	var server = net.createServer(function(socket) {
	    var date = new Date();
	    var year = date.getFullYear();
	    var month = date.getMonth() + 1;
	    month = month < 10 ? "0" + month : month;
	    var day = date.getDate();
	    day = day < 10 ? "0" + day : day;
	    var hour = date.getHours();
	    hour = hour < 10 ? "0" + hour : hour;
	    var minute = date.getMinutes();
	    minute = minute < 10 ? "0" + minute : minute;
	    var data = null;
	    data = year + '-' + month + '-' + day + ' ' + hour + ':' + minute + '\n';
	    socket.end(data); 
	});
	server.listen(process.argv[2]);  

	//  var net = require('net')   
    //  function zeroFill(i) {  
    //   return (i < 10 ? '0' : '') + i  
    //  }    
    //  function now () {  
    //   var d = new Date()  
    //   return d.getFullYear() + '-'  
    //      + zeroFill(d.getMonth() + 1) + '-'  
    //      + zeroFill(d.getDate()) + ' '  
    //      + zeroFill(d.getHours()) + ':'  
    //      + zeroFill(d.getMinutes())  
    //  }  
    //  var server = net.createServer(function (socket) {  
    //   socket.end(now() + '\n')  
    //  })   
    //  server.listen(Number(process.argv[2])) 
11.**HTTP FILE SERVER**   
QUESTION:  
　　编写一个 HTTP 文件 服务器，它用于将每次所请求的文件返回给客户端。你的服务器需要监听所提供给你的第一个命令行参数所制定的端口。  
　　同时，第二个会提供给你的程序的参数则是所需要响应的文本文件的位置。在这一题中，你必须使用 fs.createReadStream() 方法以 stream 的形式作出请求响应。  
TIPS:  
　　由于我们需要创建的是一个 HTTP 服务而不是普通的 TCP 服务，因此，你应该使用http 这个 Node 核心模块。它和 net 模块类似，http 模块拥有一个叫做http.createServer() 的方法，所不同的是它所创建的服务器是用 HTTP协议进行通信的。  
   
　　http.createServer()接收一个回调函数作为参数，回调函数会在你的服务器每一次进行连接的时候执行，这个回调函数有以下的特征：  
   
     function callback (request, response) { /* ... */ }  
   
　　在这里，这两个参数是代表一个 HTTP 请求以及相应的响应的两个对象。request用来从请求中获取一些的属性，例如请求头和查询字符（query-string)，而response 会发送数据给客户端，包括响应头部和响应主体。  
   
　　request 和 response 也都是 Nodestream！这意味着，如果需要的话，你可以使用流式处理（streaming）所抽象的那些方法来实现发送和接收数据。  
   
　　http.createServer() 会返回一个 HTTP 服务器的实例。你需要调用server.listen(portNumber) 方法去监听一个特定的端口。  
   
　　一个典型的 Node HTTP 服务器将会是这个样子：  
   
     var http = require('http')  
     var server = http.createServer(function (req, res) {  
       // 处理请求的逻辑...  
     })  
     server.listen(8000)    
　　fs 这个核心模块也含有一些用来处理文件的流式（stream） API。你可以使用fs.createReadStream() 方法来为命令行参数指定的文件创建一个stream。这个方法会返回一个 stream 对象，该对象可以使用类似 src.pipe(dst)的语法把数据从 src流传输(pipe) 到dst流中。通过这种形式，你可以轻松地把一个文件系统的 stream 和一个 HTTP响应的 stream 连接起来。  
file.js:   

	var http = require('http');  
    var fs = require('fs'); 
       
    var server = http.createServer(function (req, res) {  
       	res.writeHead(200, { 'content-type': 'text/plain' });  
       
       	fs.createReadStream(process.argv[3]).pipe(res);  
    })  
       
    server.listen(Number(process.argv[2]));
12.**HTTP UPPERCASERER**   
QUESTION:  
　　编写一个 HTTP 服务器，它只接受 POST 形式的请求，并且将 POST请求主体（body）所带的字符转换成大写形式，然后返回给客户端。  
   
  你的服务器需要监听由第一个命令行参数所指定的端口。
TIPS:  
　　through2-map 允许你创建一个 transformstream，它仅需要一个函数就能完成「接收一个数据块，处理完后返回这个数据块的功能 ，它的工作模式类似于 Array#map()，但是是针对 stream 的：  
   
     var map = require('through2-map')  
     inStream.pipe(map(function (chunk) {  
       return chunk.toString().split('').reverse().join('')  
     })).pipe(outStream)  
   
　　在上面的例子中，从 inStream传进来的数据会被转换成字符串（如果它不是字符串的话），并且字符会反转处理，然后传入outStream。所以，我们这里是做了一个字符串反转器！记住！尽管，数据块（chunk）的大小是由上游（up-stream）所决定的，但是你还是可以在这之上对传进来的数据做一点小小的处理的。  
uppercase.js:  

	var http = require("http");
	var map = require('through2-map');
	
	var server = http.createServer(function(req, res) {
	    if (req.method !== "POST") {
	        return res.end('send me a POST')
	    }
	    req.pipe(map(function (chunk) {  
	       return chunk.toString().toUpperCase();  
	    })).pipe(res); 
	});
	server.listen(process.argv[2]);

13.**HTTP JSON API SERVER**   
QUESTION:  
　　编写一个 HTTP 服务器，每当接收到一个路径为 '/api/parsetime' 的 GET请求的时候，响应一些 JSON 数据。我们期望请求会包含一个查询参数（querystring），key 是 "iso"，值是 ISO 格式的时间。  
   
如:  
	
	/api/parsetime?iso=2013-08-10T12:10:15.474Z  
   
所响应的 JSON 应该只包含三个属性：'hour'，'minute' 和 'second'。例如：  
   
     {  
       "hour": 14,  
       "minute": 23,  
       "second": 15  
     }  
   
　　然后增再加一个接口，路径为'/api/unixtime'，它可以接收相同的查询参数（querystring），但是它的返回会包含一个属性：'unixtime'，相应值是一个 UNIX时间戳。例如:  
   
     { "unixtime": 1376136615474 }  
   
　　你的服务器需要监听第一个命令行参数所指定的端口。  
TIPS:  
　　HTTP 服务器的 request 对象含有一个 url属性，你可以通过它来决定具体需要走哪一条"路由"。  
   
　　你可以使用 Node 的核心模块 'url' 来处理 URL 和 查询参数（query string）。`url.parse(request.url, true)` 方法会处理request.url，它返回的对象中包含了一些很有帮助的属性，方便你处理querystring。  
　　你的响应应该是一个 JSON 字符串的形式。请查看 JSON.stringify()来获取更多信息。  
   
　　你也应当争做 Web 世界的好公民，正确地为响应设置 Content-Type 属性：  
   
     res.writeHead(200, { 'Content-Type': 'application/json' })  
   
　　JavaScript 的 Date 可以将日期以 ISO 的格式展现出来，如：`newDate().toISOString()`。并且，如果你把一个字符串传给Date的构造函数，它也可以帮你将字符串处理成日期类型。另外，`Date.getTime()`方法应该也会很有用。  
json.js:  

	var http = require('http')  
	var url = require('url')  
	
	function parsetime (time) {  
	    return {  
	        hour: time.getHours(),  
	        minute: time.getMinutes(),  
	        second: time.getSeconds()  
	    }  
	}  
	
	function unixtime (time) {  
	    return { unixtime : time.getTime() }  
	}  
	
	var server = http.createServer(function (req, res) {  
	    var parsedUrl = url.parse(req.url, true)  
	    var time = new Date(parsedUrl.query.iso)  
	    var result  
	
	    if (/^\/api\/parsetime/.test(req.url))  
	        result = parsetime(time)  
	    else if (/^\/api\/unixtime/.test(req.url))  
	        result = unixtime(time)  
	
	    if (result) {  
	        res.writeHead(200, { 'Content-Type': 'application/json' })  
	        res.end(JSON.stringify(result))  
	    } else {  
	        res.writeHead(404)  
	        res.end()  
	    }  
	})  
	server.listen(Number(process.argv[2]))  