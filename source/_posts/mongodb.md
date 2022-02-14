---
title: MongoDB入门
date: 2021-11-12 18:34:12
description: 基于分布式文件存储的数据库
tags:
    - mongodb
categories: 数据库
---

### 可视化工具

[Robo 3T 下载地址](https://robomongo.org/download)
### mac 终端安装
```
# 进入 /usr/local
cd /usr/local

# 下载
sudo curl -O https://fastdl.mongodb.org/osx/mongodb-osx-ssl-x86_64-4.0.9.tgz

# 解压
sudo tar -zxvf mongodb-osx-ssl-x86_64-4.0.9.tgz

# 重命名为 mongodb 目录

sudo mv mongodb-osx-x86_64-4.0.9/ mongodb
```
### 全局环境
```
# 打开配置环境终端
open -e .bash_profile

# 添加 PATH 环境变量
export PATH=/usr/local/mongodb/bin:$PATH

# 刷新配置
source .bash_profile
```
### 常用命令


##### 1.设置存储目录
```
# 进入 /usr/local
cd /usr/local

# 新增数据存储目录
sudo mkdir -p /usr/local/var/mongodb

# 设置数据存放目录
mongod --dbpath /usr/local/var/mongodb
```
##### 2.设置日志目录
```
# 进入 /usr/local
cd /usr/local

# 新增数据存储目录
sudo mkdir -p /usr/local/var/mongodb

# 设置数据存放目录
mongod --dbpath /usr/local/var/log/mongodb/mongo.log
```
##### 3.连接mongodb
```
# 连接mongodb
mongo
```
##### 4.操作数据库
```
# 查询所有数据库
show dbs

# 新增/切换 blog 数据库
use blog

# 删除数据库（首先切换到要删除的数据库）
db.dropDatabase()
```
##### 5.操作表
```
# 查询所有表
show collections

# 创建表
db.createCollection("表名")

# 删除表
db.collection.drop()
```
##### 6.操作数据
注: collection为表名
```
# 查询数据 
# query: 查询条件
db.collection.find(query)

# 查询所有数据
db.collection.find()

# 查询条件数据
# AND条件
db.collection.find({ key1:value1, key2:value2 })

# OR条件
db.collection.find({ 
	$or: [
		{key1: value1}, {key2:value2}
	]
})

# 联合查询
db.collection.find({ key1:value1, $or: [{ key2:value2, key3:value3 }]})

# 新增数据
db.collection.insert({ key:value })

# 更新数据
# query : update的查询条件，类似sql update查询内where后面的。
# update : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
# upsert : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
# multi : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
# writeConcern :可选，抛出异常的级别。
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)

# 更新单个数据
db.collection.updateOne(<query>)

# 更新多个数据
db.collection.updateMany(<query>)

# 删除数据
# query :（可选）删除的文档的条件。
# justOne : （可选）如果设为 true 或 1，则只删除一个文档，如果不设置该参数，或使用默认值 false，则删除所有匹配条件的文档。
# writeConcern :（可选）抛出异常的级别。

# 2.6版本以前
db.collection.remove(
   <query>,
   <justOne>
)

# 2.6版本以后
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)

# 删除一条数据
db.collection.deleteOne(<query>)

# 删除多条数据
db.collection.deleteMany(<query>)
```
查询条件语句：

| 等于 | {<key>:<value>} |
| --- | --- |
| 小于 | {<key>:{$lt:<value>}} |
| 小于等于 | {<key>:{$lte:<value>}} |
| 大于 | {<key>:{$gt:<value>}} |
| 大于等于 | {<key>:{$gte:<value>}} |
| 不等于 | {<key>:{$ne:<value>}} |

##### 7.操作符
```
# 根据数据类型查询 操作符$type
# 例子🌰: 查询title为字符串的数据
db.col.find({"title" : {$type : 'string'}})
```
##### 8.方法
```
# NUMBER: 条数

# limit方法 读取的条数
db.collection.find().limit(NUMBER)

# skip方法 跳过的条数
db.collection.find().limit(NUMBER).skip(NUMBER)

# 例子🌰: 每页10条，查询第3页
db.collection.find().limit(10).skip(20)

# sort方法 排序
# 1: 升序 -1: 降序
db.collection.find().sort({<key>:<1或-1>})

# count方法 总条数
db.collection.find().count()
```
注：skip(), limilt(), sort()三个放在一起执行的时候，执行的顺序是先 sort(), 然后是 skip()，最后是显示的 limit()
##### 9.聚合查询
```
# aggregate方法
db.collection.aggregate([{$group : {<new_key> : "$<key>", <nwe_key> : {$sum : 1}}}])
```
| $sum | 计算总和 | db.collection.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : "$likes"}}}]) |
| --- | --- | --- |
| $avg | 计算平均值 | db.collection.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}]) |
| $min | ​最小值 | db.collection.aggregate([{$group : {_id : "$by_user", num_tutorial : {$min : "$likes"}}}]) |
| $max | 最大值 | db.collection.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}]) |
| $push | 将值加入一个数组中，不会判断是否有重复的值 | db.collection.aggregate([{$group : {_id : "$by_user", num_tutorial : {
$push: "$likes"}}}]) |
| $addToSet | 将值加入一个数组中，会判断是否有重复的值，若相同的值在数组中已经存在了，则不加入 | db.collection.aggregate([{$group : {_id : "$by_user", num_tutorial : {
$addToSet : "$likes"}}}]) |
| $first | 根据资源文档的排序获取第一个文档数据 | db.collection.aggregate([{$group : {_id : "$by_user", num_tutorial : {
$first : "$likes"}}}]) |
| $last | 根据资源文档的排序获取最后一个文档数据 | db.collection.aggregate([{$group : {_id : "$by_user", num_tutorial : {
$last : "$likes"}}}]) |

