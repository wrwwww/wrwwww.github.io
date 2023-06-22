---
title: mysql
date: 2023-05-05 21:30:42
tags:  
---

```mysql
// 创建用户
create user "用户名"@"localhost" identified by "123";
// 查看全部用户
select user from user;
// 删除用户
delete from user where user = "user1"
 


```

```mysql
// 设置权限
GRANT ALL ON *.* TO 'user'@'host';  # *.* 表示数据库库的所有库和表，对应权限存储在mysql.user表中
```
![[Pasted image 20230428183455.png]]
然后就可以使用用户创建数据库
```mysql
create database informatrion_management;
```
![[Pasted image 20230428183536.png]]
查看全部数据库
```mysql
show databases;
```
![[Pasted image 20230428183606.png]]
