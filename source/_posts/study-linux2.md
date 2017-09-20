---
title: 学习linux（二）
categories: linux
tags: [linux]
---
最近在[慕课网](http://t.imooc.com/learn/175)学习linux，写这个学习笔记主要是记录学习的过程，以及方便以后查询。  
## 三、文件搜索命令
**locate**  

	locate 文件名
	在后台数据库中按文件名搜索，搜索速度更快。

locate所搜索的后台数据库为`/var/lib/mlocate`，每天自动更新，可以使用`updatedb`强制更新。  
`/etc/updatedb.conf`为其配置文件，其中  

- 第一行表示开启搜索限制
- 第二行表示搜索时，不搜索的文件系统
- 第三行表示搜索时，不搜索的文件类型
- 第四行表示搜索时，不搜索的路径   

**whereis**  

	whereis [选项] 命令名
	得到搜索命令所在路径及帮助文档所在位置
	-b  只查找可执行文件
	-m  只查找帮助文档

**which**

	which 命令名
	搜索命令所在路径及别名
  
**find**  

	find [搜索范围] [搜索条件]
	find /root -name install.log     	表示在/root中按name匹配到install.log
	find /root -iname install.log    	忽略大小写
	find /root -user root            	按照所有者搜索
	find /root -nouser               	搜索没有所有者的文件

	find /var/log/ -mtime +10        	查找10天前修改的文件
				    atime            	文件访问的时间
					ctime            	改变文件属性的时间
					mtime			 	修改文件内容的时间
						  -10		 	10天内修改的文件
						   10		 	10天当天修改的文件
						  +10		 	10天前修改的文件

	find . size +25k				 	查找当前目录文件大于25k的文件
				-25k					 			  小于
				 25k							   	  等于

	find . inum 222222				 	查找i节点为222222的文件

	find /etc -size +20k -a -size -50k  查找大于20k且小于50k的文件
	find /etc -size +20k -o -size -10k  查找大于20k或小于10k的文件     
	
	第一个命令 -exec 第二个命令 {} \;     对第一个命令得到的数据进行第二个命令操作
	find /etc -size +20k -a -size -50k -exec ls -lh {} \; 显示大于20k且小于50k的文件的详细信息

**grep**  

	grep [选项] 字符串 文件名             在文件名中匹配符合条件的字符串
		 -i   							忽略大小写
		 -v     							排除指定的字符串

**find与grep的区别**  
find命令主要是搜索符合条件的文件名，如果需要匹配，使用通配符，是完全匹配  
grep命令主要是搜索符合条件的字符串，如果需要匹配，使用正则表达式进行匹配，是包含匹配。

## 四、帮助命令  
**man**  

	man 命令                            该命令使用q退出
	命令 --help

	help shell内部命令
	wherris 命令                        确定是否为内部命令
	help 命令

	info 命令                           列出所有命令清单

**压缩**  

	zip 压缩文件名 原文件                压缩文件
	zip -r 压缩文件名 原目录             压缩目录
	unzip 压缩文件

	gzip 原文件                         压缩为.gz的文件，原文件消失
	gzip -c 原文件 > 压缩文件            保留原文件	
	gzip -r 目录                        压缩目录下的所有子文件
	gzip -d 压缩文件                     解压
	gunzip 压缩文件                     解压缩文件

	bzip2压缩不能压缩目录
	bzip2 源文件                        压缩不保留源文件
	bzip2 -k 源文件                     保留原文件
	bzip2 -dk 压缩文件                  解压缩-k保留压缩文件
	bunzip2 -k 压缩文件

	tar -cvf 打包文件名 源文件
		-c 打包
		-v 显示过程
		-f 指定打包后的文件
	tar -xvf 打包文件名
		-x   解打包
	tar -zcvf 压缩包名.tar.gz 源文件
		-z   压缩为.tar.bz2格式
	tar -zxvf 压缩包名.tar.gz
	tar -jcvf 后缀为tar.bz2

**关机与重启命令**  

	shutdown [选项] 时间
			 -c 取消到前一个关机命令
			 -h 关机
			 -r 重启

	halt
	poweroff
	init 0

	reboot
	init 6

	logout  养成退出登录的习惯