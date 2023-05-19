---
title: mongoDB
date: 2023-05-05 21:30:42
tags:  
---
# mongoDB的安装

## 配置文件mongod.cfg
```yaml
 
storage:
	数据库的路径
  dbPath: D:\software\mongodb-win32-x86_64-windows-6.0.4\data\db
  journal:
    enabled: true
 
systemLog:
  destination: file
  logAppend: true
	  日志的路径
  path:  D:\software\mongodb-win32-x86_64-windows-6.0.4\log\mongod.log
net:
  port: 27017
  bindIp: 127.0.0.1
```
目录的创建 
创建数据库路径和log路径
```shell
mongod --dbpath 上面的数据库路径
刷入MongoDB服务
mongod --config '绝对路径的配置文件' --install 
net start MongoDB 开启服务
net stop MongoDB 关闭服务
mongod --remove 删除服务
```

## MongoDB的使用
```shell
全部数据库
show dbs/databases
当前数据库
db
创建和使用数据库
use db 
当前数据库中的表
show collocation
查看指定表中的数据
db.getCollection("指定表").find({name:"sss"}).pretty()
等同于
db.users.find({}).pretty()
pretty()一行打印

更新数据库
db.users.update({"查询的条件"},{$set{'更新的内容'}})
db.users.update({username:'admin'},{$set:{ isAdmin : true }})

插入数据
db.users.insert({}) //如果主键重复抛出异常,提示主键重复,数据不被保存
db.users.save({}) // 如果主键存在更新数据,否则插入数据,已废弃可以使用 insertOne()或者replaceOne()替换

删除数据
db.users.deleteOne({条件})
删除所有
db.users.deleteMany({})
查看所有
db.users.find()

逻辑运算符 与或非
and 的话就是多个条件
db.users.find({条件1,条件2})

or
db.users.find($or:[{条件1},{条件2}])

and or
dn.users.find({like:'ss',$or:[{条件1},{条件2}]})

```
