---
title: xampp和MySQL配置环境时遇到的问题
categories: 后台
tags: 后台
---
### xampp和MySQL配置环境时遇到的问题    
查看端口被占用  

	netstat -ano|findstr "<端口号>"  

> access forbidden  
> you don't have permission to access the requested directory
  
在xampp\apache\conf\httpd.conf中的`<Directory> blocks below.`下面加入

	<Directory />
    	AllowOverride none
    	Require all denied
	</Directory>  
  
在navicat中运行sql文件时报错
> MySQL Error 2013: Lost connection to MySQL server during query  
  
在my.ini文件中`The MySQL server`下面增加  

	wait_timeout=86400
	interactive_timeout=86400 
并将`max_allowed_packet = 16M` 改为 `max_allowed_packet = 500M`