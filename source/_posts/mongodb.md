---
title: mongodb安装以及启动服务
categories: mongodb
tags: mongodb
---
### mongodb
首先安装mongodb，[官方下载](https://www.mongodb.com/download-center?jmp=nav#community)，然后直接安装，获取安装文件夹bin所在目录将其添加到系统变量中。使用`mongod -v`来验证是否添加成功。  
安装好mongodb后直接输入指令`mongo`会报错，`errno:10061 由于目标计算机积极拒绝，无法连接`  
原因在于未添加配置文件，步骤如下：  
1.在所需文件夹下创建数据目录`mkdir data`，创建日志目录`mkdir log`;  
2.使用mongo命令来启动数据服务`mongod --dbpath data --logpath log\mongod.log --logappend`  
语法为：   
 	
	mongod --dbpath $dbpath 
	--logpath $logpath 
	--logappend 
	--fork

3.在另一个终端中使用`mongo`来连接数据库  

------
为了方便数据库的管理，我们可以将mongod命令添加到windows的系统服务中  
安装数据库服务：  

	mongod --dbpath "C:\Users\Think\Desktop\nodejs+mongodb\data" --logpath "C:\Users\Think\Desktop\nodejs+mongodb\log\mongod.log" --logappend --install --serviceName "imooc"  

可以使用`net start imooc`命令来启动数据库服务，`net stop imooc`命令关闭数据库服务。  
语法为： 
 
	mongod --dbpath "$dbpath"
	--logpath "$logpath" --logappend
	--install [remove]
	--serviceName "imooc"
	
	net strat/stop imooc