---
title: webpack从0搭建学习笔记  
categories: webpack
tags: [webpack]
---
之前经常使用vue-cli去构建一个vue应用，但是很少单独使用webpack，所以打算系统性的学习一下webpack。  

### webpack核心概念
- `entry` 一个可执行模块或库的入口文件。
- `chunk` 多个文件组成的一个代码块，例如把一个可执行模块和它所有依赖的模块组合和一个 `chunk` 这体现了webpack的打包机制。
- `loader` 文件转换器，例如把es6转换为es5，scss转换为css。
- `plugin` 插件，用于扩展webpack的功能，在webpack构建生命周期的节点上加入扩展hook为webpack加入功能。

### webpack构建流程
从启动webpack构建到输出结果经历了一系列过程，它们是：  
- 解析webpack配置参数，合并从shell传入和`webpack.config.js`文件里配置的参数，生产最后的配置结果。
- 注册所有配置的插件，好让插件监听webpack构建生命周期的事件节点，以做出对应的反应。
- 从配置的`entry`入口文件开始解析文件构建AST语法树，找出每个文件所依赖的文件，递归下去。
- 在解析文件递归的过程中根据文件类型和loader配置找出合适的loader用来对文件进行转换。
- 递归完后得到每个文件的最终结果，根据`entry`配置生成代码块`chunk`。
- 输出所有`chunk`到文件系统。  

需要注意的是，在构建生命周期中有一系列插件在合适的时机做了合适的事情，比如`UglifyJsPlugin`会在loader转换递归完后对结果再使用`UglifyJs`压缩覆盖之前的结果。  

以上内容都是摘抄    

---

### 开始

安装配置
```
mkdir webpack && cd webpack
npm init -y
cnpm install webpack --save-dev
```

因为webpack需要配置参数，默认文件为`webpack.config.js`，因此，我们新建`webpack.config.js`文件。  

```
touch webpack.config.js
```

### 配置文件  

> webpack 配置的本质就是：一个配置的 Object。  

因此有两种使用方法配置 webpack:

- 一种是 Cli（即 Command Line Interface）方法：读取 `webpack.config.js` 文件；
- 另一种是 Node.js API 的方法：传递配置对象作为参数。  

前端一般是使用第一种，即配置 `webpack.config.js` 文件：  

	
	const path = require('path');
	
	module.exports = {
	    // 还有以下两种方式
	    // entry: ["./src/entry1", "./src/entry2"],
	    // entry: "./src/entry"
	    entry: {
	        index: './src/main.js'
	    },
	    output: {
	        path: path.resolve(__dirname, 'dist'),
	        // filename: "bundle.js", // string
	        // filename: "[name].js", // 用于多个入口点(entry point)（出口点？）
	        // filename: "[chunkhash].js", // 用于长效缓存
	        // 「入口分块(entry chunk)」的文件名模板（出口分块？）
	        filename: 'js/[name].bundle.js'
	    }
	}
	

如上所示，设置文件入口entry，和文件出口output。我们在根目录下新建`src/main.js`  


	//main.js
	console.log('hello, webpack');


执行命令`webpack`，可以看到在根目录下生成了`dist/js/index.bundle.js`  

但是，这样只是打包了js文件，下面我们生成一个html文件来动态引入上面生成的js文件。  

### Plugins  

> webpack 有着丰富的插件接口(rich plugin interface)。webpack 自身的多数功能都使用这个插件接口。这个插件接口使 webpack 变得极其灵活。  


#### html-webpack-plugin
这里使用`html-webpack-plugin`插件，首先安装  

	
	cnpm install html-webpack-plugin --save-dev
	
在webpack.config.js文件中引入插件，修改如下


	const path = require('path');
	const htmlWebpackPlugin = require('html-webpack-plugin');
	module.exports = {
	    ...
	    plugins: [
	        new htmlWebpackPlugin({
	            filename: 'index.html', //最终生成的存放路径，相对于outpu的path
	            template: 'index.html', //模板文件
	            inject: true, //js插入的位置，true/'head'/'body'/false
	            hash: true, //为静态资源生成hash值
	            chunks: ['vendors', 'index'], //需要引入的chunk，不配置就会引入所有页面的资源
	            minify: { //压缩HTML文件    
	                removeComments: true, //移除HTML中的注释
	                collapseWhitespace: false //删除空白符与换行符
	            }
	        })
	    ]
	}

因为在上方配置中我们引入了模板文件index.html，所以我们在根目录下新建最基本html文件，然后执行`webpack`，我们发现dist目录下新生成了一个index.html文件，而且引入了同时打包生成的js文件，用浏览器打开发现console栏打印了我们main.js里的内容。  

#### 热更新
如果修改内容后每次都要去使用`webpack`命令去打包，然后按f5刷新浏览器再看修改效果，这样太麻烦，我们使用热更新。  

首先安装

	cnpm install webpack-dev-server --save-dev

在webpack.config.js文件中引入插件，修改如下

	const webpack = require('webpack');
	const path = require('path');
	const htmlWebpackPlugin = require('html-webpack-plugin');
	module.exports = {
	    ...
	    plugins: [
	        ...
	        new webpack.HotModuleReplacementPlugin()
	    ],
	    devServer: {
	        contentBase: './dist',
	        // port: 9090, //如果不指定默认为8080，并且每次重启会+1
	        inline: true, //可以监控js变化
	        hot: true  //开启服务 热更新
	    }
	}

方便操作，修改package.json文件：

	"scripts": {
	    "test": "echo \"Error: no test specified\" && exit 1",
	    "dev": "webpack-dev-server --open --progress --colors"
	}

其中的dev为我们新增，后面的参数分别为  
- `--open` 使用默认浏览器打开url
- `--progress` 显示打包进度
- `--colors` 给部分输出文字加上颜色，方便观看

关于webpack-dev-server更详细的配置可看[webpack-dev-server CLI](http://webpack.github.io/docs/webpack-dev-server.html#webpack-dev-server-cli)  

这样执行命令`npm run dev`，就可以看到默认浏览器打开了`http://loacalhost:8080`，并且每次修改css，js等文件会实时更新，解放了每次打包和刷新的麻烦。  

### module

> webpack 通过 loader 可以支持各种语言和预处理器编写模块。loader 描述了 webpack 如何处理 非 JavaScript(non-JavaScript) 模块，并且在bundle中引入这些依赖。

#### css与sass

上文中，最终的dist/index.html是根据模板文件index.html所生成的，并且动态加入了生成的js文件，那么css文件应该怎样动态加入？需要用到各种loader。    

首先在src目录下新建src/css/main.css 

	div {
	    color: red;
	}

在模板文件index.html中加入
	
	<div>test</div>
	
想要动态加载css，首先安装
	
	cnpm install css-loader style-loader --save-dev
	
修改配置文件  
	
	...
	module.exports = {
	    ...
	    module: {
	        rules: [
	            {
	                test: /\.css$/,
	                use: [
	                    'css-loader', //用于解析
	                    'style-loader' //将解析后的样式嵌入js代码
	                ]
	            }
	        ]
	    }
	}
	
文件都配置好了，但是还没有引入css文件，所以在文件入口main.js中引入该css文件
	
	import './css/main.css';
	
这时查看浏览器发现样式已经被动态的加到html文件里的style标签里面，但是，有时候我们需要css文件为单独的文件，并不是内嵌到html里面。  

首先安装插件 extract-text-webpack-plugin
	
	cnpm install --save-dev extract-text-webpack-plugin
	
修改配置文件  
	
	...
	const ExtractTextPlugin = require('extract-text-webpack-plugin');
	...
	module.exports = {
	    ...
	    plugins: [
	        ...
	        new ExtractTextPlugin("[name]-[hash].css")
	    ],
	    module: {
	        rules: [
	            {
	                test: /\.css$/,
	                use: ExtractTextPlugin.extract(['css-loader'])
	            }
	        ]
	    }
	}
	
可以看到我们的css已经被单独提出来了。但是，实际开发中我们需要对浏览器的兼容性做处理。这时我们需要用到postcss。

首先安装插件
	
	cnpm install --save-dev postcss-loader autoprefixer
	
新建postcss.config.js
	
	module.exports = {
	    plugins : [
	        require('autoprefixer')({
	            browsers : ['last 5 versions']
	        })
	    ]
	}

	
修改配置文件
	
	module.exports = {
	    module: {
	        rules: [
	            {
	                test: /\.css$/,
	                use: ExtractTextPlugin.extract(['css-loader', 'postcss-loader'])
	            }
	        ]
	    }
	}
	
开发时，需要使用sass  
首先安装插件
	
	cnpm install sass-loader node-sass --save-dev
	
修改配置文件
	
	module.exports = {
	    module: {
	        rules: [
	            {
	                test: /\.css$/,
	                use: ExtractTextPlugin.extract(['css-loader', 'postcss-loader'])
	            },
	            {
	                test: /\.scss$/,
	                // sass-loader要在css-loader之后
	                use: ExtractTextPlugin.extract(['css-loader', 'postcss-loader', 'sass-loader'])
	            }
	        ]
	    }
	}
	
按照上述修改后，可以无痛sass开发了，但是发现个问题，此时浏览器并没有实时更新了，经过各种查找把`hot: true`屏蔽即可，原因未知

#### es6  
首先安装插件
	
	cnpm install --save-dev babel-loader babel-core babel-preset-env
	
修改配置文件
	
	module.exports = {
	    module: {
	        rules: [
	            ...
	            {
	                test: /\.js$/,
	                exclude: /(node_modules)/,
	                use: {
	                    loader: 'babel-loader',
	                    options: {
	                        presets: ['env']
	                    }
	                }
	            }
	        ]
	    }
	}
	

#### 图片处理
首先安装插件
	
	cnpm install --save-dev url-loader
	
修改配置文件
	
	module.exports = {
	    module: {
	        rules: [
	            ...
	            {
	                test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
	                loader: 'url-loader',
	                options: {
	                    limit: 10000,
	                    name: 'assets/[hash].[ext]'
	                }
	            }
	        ]
	    }
	}
	
但是，上述方案只能对css中的图片进行处理，我们还需要对html中的img标签进行处理  
	
	cnpm install --save-dev html-withimg-loader
	
修改配置文件
	
	module.exports = {
	    module: {
	        rules: [
	            ...
	            {
	                test: /\.(htm|html)$/i,
	                loader: 'html-withimg-loader' //处理html中的img标签
	            }
	        ]
	    }
	}
	
好了，上述可以满足平常的基本开发了。  

如果需要多页面开发的话，则在插件中new一个新的htmlWebpackPlugin插件。  
	
	...
	entry: {
	    index: './src/main.js',
	    a: './src/a.js'
	},
	...
	plugins: [
	    ...
	    new htmlWebpackPlugin({
	        filename: 'a.html', //最终生成的存放路径，相对于outpu的path
	        template: 'a.html', //模板文件
	        inject: true, //js插入的位置，true/'head'/'body'/false
	        hash: true, //为静态资源生成hash值
	        chunks: ['vendors', 'a'], //需要引入的chunk，不配置就会引入所有页面的资源
	        minify: { //压缩HTML文件
	            removeComments: true, //移除HTML中的注释
	            collapseWhitespace: false //删除空白符与换行符
	        }
	    }),
	    ...
	]
	...
	

该页面的入口为a.js，并且设置chunks只引入需要的。   

---
最终的package.json和webpack.config.js和postcss.config.js文件分别如下

**package.json**
	
	{
	  "name": "webpack",
	  "version": "1.0.0",
	  "description": "",
	  "main": "index.js",
	  "scripts": {
	    "test": "echo \"Error: no test specified\" && exit 1",
	    "dev": "webpack-dev-server --open --progress --colors --content-base",
	    "build": "webpack --progress --colors"
	  },
	  "keywords": [],
	  "author": "",
	  "license": "ISC",
	  "devDependencies": {
	    "autoprefixer": "^7.1.4",
	    "babel-core": "^6.26.0",
	    "babel-loader": "^7.1.2",
	    "babel-preset-env": "^1.6.0",
	    "css-loader": "^0.28.7",
	    "extract-text-webpack-plugin": "^3.0.0",
	    "html-webpack-plugin": "^2.30.1",
	    "html-withimg-loader": "^0.1.16",
	    "node-sass": "^4.5.3",
	    "postcss-loader": "^2.0.6",
	    "sass-loader": "^6.0.6",
	    "style-loader": "^0.18.2",
	    "url-loader": "^0.5.9",
	    "webpack": "^3.5.6",
	    "webpack-dev-server": "^2.8.1"
	  }
	}
	

**postcss.config.js**
	
	module.exports = {
	    plugins : [
	        require('autoprefixer')({
	            browsers : ['last 5 versions']
	        })
	    ]
	}
	

**webpack.config.js**
	
	const webpack = require('webpack');
	const path = require('path');
	const htmlWebpackPlugin = require('html-webpack-plugin');
	/*
	extract-text-webpack-plugin插件，
	有了它就可以将你的样式提取到单独的css文件里，
	妈妈再也不用担心样式会被打包到js文件里了。
	 */
	const ExtractTextPlugin = require('extract-text-webpack-plugin');
	
	module.exports = {
	    // 还有以下两种方式
	    // entry: ["./src/entry1", "./src/entry2"],
	    // entry: "./src/entry"
	    entry: {
	        index: './src/main.js',
	        a: './src/a.js'
	    },
	    output: {
	        path: path.resolve(__dirname, 'dist'),
	        // filename: "bundle.js", // string
	        // filename: "[name].js", // 用于多个入口点(entry point)（出口点？）
	        // filename: "[chunkhash].js", // 用于长效缓存
	        // 「入口分块(entry chunk)」的文件名模板（出口分块？）
	        filename: 'js/[name].bundle.js'
	    },
	    devServer: {
	        contentBase: './dist',
	        // port: 9090,
	        inline: true, //可以监控js变化
	        // hot: true  //开启服务 热更新
	    },
	    plugins: [
	        new htmlWebpackPlugin({
	            filename: 'index.html', //最终生成的存放路径，相对于outpu的path
	            template: 'index.html', //模板文件
	            inject: true, //js插入的位置，true/'head'/'body'/false
	            hash: true, //为静态资源生成hash值
	            chunks: ['vendors', 'index'], //需要引入的chunk，不配置就会引入所有页面的资源
	            minify: { //压缩HTML文件
	                removeComments: true, //移除HTML中的注释
	                collapseWhitespace: false //删除空白符与换行符
	            }
	        }),
	        new htmlWebpackPlugin({
	            filename: 'a.html', //最终生成的存放路径，相对于outpu的path
	            template: 'a.html', //模板文件
	            inject: true, //js插入的位置，true/'head'/'body'/false
	            hash: true, //为静态资源生成hash值
	            chunks: ['vendors', 'a'], //需要引入的chunk，不配置就会引入所有页面的资源
	            minify: { //压缩HTML文件
	                removeComments: true, //移除HTML中的注释
	                collapseWhitespace: false //删除空白符与换行符
	            }
	        }),
	        new ExtractTextPlugin("css/[name].css"),
	        new webpack.HotModuleReplacementPlugin()
	    ],
	    module: {
	        rules: [
	            // {
	            //     test: /\.css$/,
	            //     use: [
	            //         'css-loader', //用于解析
	            //         'style-loader' //将解析后的样式嵌入js代码
	            //         // {
	            //         //     loader: 'css-loader',
	            //         //     options: {
	            //         //         importLoaders: 1
	            //         //     }
	            //         // },
	            //         // 'postcss-loader' // 使用autoprefix
	            //     ]
	            // }
	            {
	                test: /\.css$/,
	                use: ExtractTextPlugin.extract(['css-loader', 'postcss-loader'])
	            },
	            {
	                test: /\.scss$/,
	                use: ExtractTextPlugin.extract(['css-loader', 'postcss-loader', 'sass-loader'])
	            },
	            {
	                test: /\.js$/,
	                exclude: /(node_modules)/,
	                use: {
	                    loader: 'babel-loader',
	                    options: {
	                        presets: ['env']
	                    }
	                }
	            },
	            {
	                test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
	                loader: 'url-loader',
	                options: {
	                    limit: 10000,
	                    name: 'assets/[hash].[ext]'
	                }
	            },
	            {
	                test: /\.(htm|html)$/i,
	                loader: 'html-withimg-loader' //处理html中的img标签
	            }
	        ]
	    }
	}



剩下的还有很多未完成的可以查看 [webpack官方文档](https://webpack.js.org/)
