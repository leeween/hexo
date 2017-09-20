---
title: npm command not found 解决方法
categories: nodejs
tags: npm
---
# npm command not found 解决方法

设置npm全局模块的存放路径以及cache的路径  
如我希望将以上两个文件夹放在NodeJS的主目录下，便在NodeJs下建立"node\_global"及"node\_cache"两个文件夹。  
启动命令行  

		npm config set prefix "C:\Program Files\nodejs\node_global"
		以及
		npm config set cache "C:\Program Files\nodejs\node_cache"

进入环境变量对话框，在系统变量下新建"NODE_PATH"，输入”C:\Program Files\nodejs\node_global\node_modules“。（ps：这一步相当关键。）  

由于改变了module的默认地址，所以上面的用户变量都要跟着改变一下（用户变量"PATH"修改为“C:\Program Files\nodejs\node_global\”），要不，使用module的时候会导致输入命令出现“xxx不是内部或外部命令，也不是可运行的程序或批处理文件”这个错误。