---
title: 学习MongoDB（一）
categories: MongoDB
tags: [MongoDB]
---
### 一、数据库  
- 一个mongoDB可以创建多个数据库
- 使用show dbs可以查看所有数据库的列表
- 运行use命令可以连接到指定的数据库
- 执行db命令则可以查看当前数据库对象或者集合  

数据库的信息存储在集合中，他们统一使用系统的命名空间：DBNAME.system.*， 其中DBNAME可用db或数据库名替代

- DBNAME.system.namespaces ：列出所有名字空间
- DBNAME.system.indexs ：列出所有索引
- DBNAME.system.profile ：列出数据库概要信息
- DBNAME.system.users ：列出访问数据库的用户
- DBNAME.system.sources ：列出服务器信息

### 二、数据库的创建和销毁  
#### 创建
启动服务后，进入 MongoDB 命令行操作界面：  
`$ mongo`  
使用 use 命令创建数据库：  
`> use mydb`  
查看当前连接的数据库：  
`> db`  
查看所有的数据库：  
`> show dbs`   
列出的所有数据库中看不到 mydb或者显示mydb(empty) ，因为 mydb 为空，里面没有任何东西，MongoDB不显示或显示mydb(empty)。  

#### 销毁  
使用 db.dropDatabase() 销毁数据库：
	
	> use local
	 switched to db local
	> db.dropDatabase()
	
查看所有的数据库：  
`> show dbs`  

### 三、集合（collection）的创建和删除  
在数据库 mydb 中创建一个集合

	> use mydb
	switched to db mydb
	> db.createCollection("users")
查看创建的集合：

`> show collections`  
删除集合的方法如下：（删除 users 集合）

	> show collections
	> db.users.drop()
查看是否删除成功：

`> show collections`

向集合中插入数据使用`insert()`和`save()`  

### 四、数据查询  
find() 用法：`db.COLLECTION_NAME.find()`  

查询数据，不加任何参数默认返回所有数据记录：  
`> db.post.find()`   

查询数据，不加任何参数默认返回所有数据记录：  
`> db.post.find()`  

#### AND
当 find() 中传入多个键值对时，MongoDB就会将其作为 AND 查询处理。用法：`db.COLLECTION_NAME.find({ key1: value1, key2: value2 }).pretty()`   

#### OR
MongoDB中，OR 查询语句以 $or 作为关键词，用法如下：

	> db.post.find(
	   {
	      $or: [
	         {key1: value1}, {key2:value2}
	      ]
	   }
	).pretty()   

#### 同时使用AND和OR
	> db.post.find({
	    "likes": {$gt:10},
	    $or: [
	        {"by": "shiyanlou"},
	        {"title": "MongoDB Overview"}
	    ]
	}).pretty()
{\$gt:10} 表示大于10，另外，\$lt 表示小于，\$lte 表示小于等于，\$gte 表示大于等于，\$ne 表示不等于。