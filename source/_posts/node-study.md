---
title: nodejs学习笔记(1)
categories: nodejs
tags: nodejs
---
# [freecodecamp](https://www.freecodecamp.com)之node学习笔记 
----------
### node安装
1. [nodejs官网](https://nodejs.org/ "nodejs官网") 直接下载LTS长期支持版本  
2. `node -v`查看nodejs版本号，`npm -v`查看npm版本号来确认是否安装成功。npm为nodejs包管理工具，安装nodejs会自动安装npm。  

### Manage Packages with NPM
> 内容出自[how-to-npm](https://github.com/npm/how-to-npm)并在[c9]( http://c9.io)上运行  

1. Install npm  
安装npm: 　`npm install npm -g`，上面提到nodejs会自动安装npm，因此此处操作实为更新。  
2. Dev Environment  
3. Login  
上述两步为新建开发文件夹以及模拟注册登录方便发布package  
4. Start A Project  
`npm init` 引导你在当前文件夹下创建一个package.json文件，包括名称、版本、作者这些信息等，例如：

		{
		  "name": "@lee/chat-example",
		  "version": "0.0.0",
		  "description": "A chat example to showcase how to use `socket.io` with a static `express` server with `async` for control flow.",
		  "main": "server.js",
		  "author": "Mostafa Eweda <mo.eweda@gmail.com>",
		  "dependencies": {
		    "async": "~0.2.8",
		    "express": "~3.2.4",
		    "socket.io": "~0.9.14"
		  },
		  "scripts": {
		    "start": "node server.js"
		  },
		  "devDependencies": {},
		  "license": "ISC"
		}  

5. Install A Module  
`npm install <modulename>`,会在当前开发环境下下载module，如果加入参数 `-g` 则会在全局环境中安装该module，例如：
`npm install @linclark/pkg`  
6. Listing Dependencies  
`npm ls`该命令可以查看项目中文件结构。如果按照上述步骤下来，此处会报错`npm ERR! extraneous: @linclark/pkg@1.0.2 /home/ubuntu/workspace/node_modules/@linclark/pkg`问题在于第五步，我们安装模块后并没有将其关联到项目中，因此我们需要在package.json中的dependencies添加依赖。运行`npm install @linclark/pkg --save`命令，此处`--save`代表安装该模块后再配置文件package.json文件中添加依赖，此时的package.json文件如下：  
		
		{
		  "name": "leeween",
		  "version": "0.0.0",
		  "description": "A chat example to showcase how to use `socket.io` with a static `express` server with `async` for control flow.",
		  "main": "server.js",
		  "author": "Mostafa Eweda <mo.eweda@gmail.com>",
		  "dependencies": {
		    "@linclark/pkg": "^1.0.2",
		    "async": "~0.2.8",
		    "express": "~3.2.4",
		    "socket.io": "~0.9.14"
		  },
		  "scripts": {
		    "start": "node server.js"
		  },
		  "devDependencies": {},
		  "license": "MIT",
		  "keywords": [
		    "an",
		    "example"
		  ]
		}
7. npm Test  
新建test.js文件，随便打印点东西 `console.log("good job")`  
并在package.json中的scripts中添加`"test": "node test.js"`此时的package.json文件如下：

		{
		  "name": "leeween",
		  "version": "0.0.0",
		  "description": "A chat example to showcase how to use `socket.io` with a static `express` server with `async` for control flow.",
		  "main": "server.js",
		  "author": "Mostafa Eweda <mo.eweda@gmail.com>",
		  "dependencies": {
		    "@linclark/pkg": "^1.0.2", 
		    "async": "~0.2.8",
		    "express": "~3.2.4",
		    "socket.io": "~0.9.14"
		  },
		  "scripts": {
		    "start": "node server.js",
		    "test": "node test.js"
		  },
		  "devDependencies": {},
		  "license": "MIT",
		  "keywords": [
		    "an",
		    "example"
		  ]
		}
运行`node test.js` 会发现打印了good job  
8. Package Niceties  
我们创建的package.json没有怎么填信息，所以当别人install下来的时候会直接懵逼，所以我们应该添加一个README.md文件来添加说明，最好将一下重要信息填在package.json中。  
9. Publish  
`npm publish` 发布你的module，`npm view <modulename>`可以查看名为modulename的module内容，同时也可以检测该名称是否被用。  
10. Version  
npm中的每个package都拥有一个版本号，比如说1.2.3 其中3是指Patch version. Update for every change，2是指Minor version. Update for API additions，1是指Major version. Update for breaking API changes.  
`npm version <versionnum>`更新版本号至versionnum。  
11. Dist Tag  
12. Dist Tag Removal  
how-to-npm中的11和12可能有bug，添加dist-tag一直404，官方[github issue45](https://github.com/npm/how-to-npm/issues/45)上也是似是而非，在此不做深究。语法如下： 

		npm dist-tag add <pkg>@<version> [<tag>]
        npm dist-tag rm <pkg> <tag>
        npm dist-tag ls [<pkg>]

        aliases: dist-tags 
