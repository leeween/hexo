---
title: nodejs学习笔记(3)——express
categories: nodejs
tags: nodejs
---
# [freecodecamp](https://www.freecodecamp.com)之node学习笔记(3)——express 
---------- 
### Build Web Apps with Expressjs
> 内容出自[expressworks](https://github.com/azat-co/expressworks)并在[c9]( http://c9.io)上运行   

1.**HELLO WORLD!**   
QUESTION:  
创建一个Express.js文件的express的实例对象app,当访问`'/home'`时,输出`'Hello World!'`
需要监听的端口号将会作为第一个参数传递给你的程序。  
hello.js  

	var express = require("express");
	var app = express();
	app.get('/home', function(req, res){
	    res.end("Hello World!");
	})
	app.listen(process.argv[2])  
2.**STATIC**   
QUESTION:  
在不使用路由的情况下，应用静态中间层来打开index.html。  
需要监听的端口号将会作为第一个参数传递给你的程序，index.html文件作为第二个参数传递给你的程序。  
TIPS:  
应用静态中间层的方法：  
	
	app.use(express.static(path.join(__dirname, 'public')));
在本题中可以这样使用：  

	app.use(express.static(process.argv[3]||path.join(__dirname, 'public')));
static.js  
	
	var express = require("express");
	var path = require("path");
	var app = express();
	
	app.use(express.static(process.argv[3] || path.join(__dirname, 'public')));
	app.listen(process.argv[2]);  
3.**JADE**  
QUESTION: 
创建一个Express.js文件的express的实例对象app，它的主页由jade模板引擎渲染。  
主页在`'/home'`，展示数据时需要使用`toDateString`  
TIPS:   
当文件夹名为'templates'时，在express.js中指定路径

	 app.set('views', path.join(__dirname, 'templates')) 
在本题中'index.jade'作为第二个参数提供。  
指出app使用的模板引擎  

	 app.set('view engine', 'jade')
`res.render()`接收模板名和展示出模板数据 

	res.render('index', {date: new Date().toDateString()})

jade.js:  
	
	var express = require('express');
    var app = express();
    app.set('view engine', 'jade');
    app.set('views', process.argv[3]);
    app.get('/home', function(req, res) {
      res.render('index', {date: new Date().toDateString()})
    });
    app.listen(process.argv[2]);
4.**GOOD OLD FORM**  
QUESTION:  
编写一个路由`'/form'`来管理HTML表单的input`<form><input name="str"/></form>)`并且打印出str的值。  
TIPS:  
处理post请求使用`post()`方法和`get()`方法类似。
Express.js用中间层来对的web server提供额外的功能。简言之，在你自己处理请求前中间层就是函数的调用。中间层提供了种类繁多的功能，例如logging,serving,static files以及error handling。在应用中用`use()`来添加中间层和转化中间层为参数。  
Express.js用`body-parser`这个模块中的`urlencoded()`作为中间层来转化请求中的`x-www-form-urlencoded`

	var bodyparser = require('body-parser')
    app.use(bodyparser.urlencoded({extended: false}))
我们可以这样翻转字符串  
	
	req.body.str.split('').reverse().join('')       
good.js:  
	
	var express = require("express");
	var bodyParser = require("body-parser");
	var app = express();
	app.use(bodyParser.urlencoded({extended: false}));
	app.post('/form', function(req, res) {
	    var str = req.body.str.split('').reverse().join('');
	    res.end(str);
	});
	app.listen(process.argv[2]);
5.**STYLISH CSS**   
QUESTION:  
Style your HTML from previous example with some Stylus middleware.

Your solution must listen on the port number supplied by process.argv[2].

The path containing the HTML and Stylus files is provided in process.argv[3]
(they are in the same directory).  
TIPS:  
To plug-in stylus someone can use this middleware:

    app.use(require('stylus').middleware(__dirname + '/public'));

Remember that you need also to serve static files.  
stylish.js  

	var express = require('express');
	var stylus = require("stylus");
	
	var app = express();
	app.use(stylus.middleware(process.argv[3]));
	app.use(express.static(process.argv[3]));
	
	app.listen(process.argv[2]);
6.**PARAM PAM PAM**   
QUESTION:  
Create an Express.js server that processes PUT '/message/:id' requests.For instance:

    PUT /message/526aa677a8ceb64569c9d4fb
TIPS:  
As a response to these requests, return the SHA1 hash of the current date
plus the sent ID:

    require('crypto')
      .createHash('sha1')
      .update(new Date().toDateString() + id)
      .digest('hex') 
To handle PUT requests use:

    app.put('/path/:NAME', function(req, res){...});

To extract parameters from within the request handlers, use:

    req.params.NAME
pam.js  
	
	var express= require("express");
	var crypto = require("crypto");
	var app = express();
	
	app.put('/message/:NAME', function(req, res){
	    var id = req.params.NAME;
	    res.send(crypto
	        .createHash('sha1')
	        .update(new Date().toDateString() + id)
	        .digest('hex'))
	})
	app.listen(process.argv[2])  